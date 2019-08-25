---
layout: post
title: Yummy spreadsheets
date: '2019-08-24 21:54:00'
tags:
  - typescript
  - nodejs
  - google-sheets
  - serverless
  - cooking
feature-img: /assets/img/yummy-spreadsheets/cover.jpg
---

**_Tl;dr_** _I made a little service to create a supermarket-aisle-sorted, quantity-merged shopping list for my spreadsheet of recipes (and you can use it too!)_

### The pitch

There’s no side project more satisfying than one that optimises my day-to-day life in some even very small way.

It doesn’t matter to me if hundreds of similar solutions already exist and many are well designed and available for free. None of them will ever work in _exactly the way I want them to_ and integrate seamlessly with my current habits.\*

\*_I’d like to make it clear to any potential employers that this is a philosophy I apply to side projects but not in my professional work where I aim to be unflinchingly pragmatic._

### I <3 spreadsheets

As anyone who’s ever shared a residence with me will tell you, I love a good spreadsheet. Arguably I’ve made spreadsheets to solve problems I don’t really have, but I take a lot of joy in them all the same.

![my adventure time watch record spreadsheet](/assets/img/yummy-spreadsheets/adventure-time-spreadsheet.png){: class="edgeless-image"}
(case in point: my adventure time watch record spreadsheet)

Perhaps my favourite is my _recipe spreadsheet_, which has matured over the past few years, and is a nice way for me to record all the dinners I’ve made that I want to remember for another time (and those I should remember to avoid).

### Inconvenience food

However, every time I want to plan a week of meals I have to do the following:

- Click through to each of my chosen recipes and for each one close on average maybe 4 obstructive adverts and/or privacy policy banners
- Find the list of ingredients on each page (not always as easy as you might imagine)
- Manually merge ingredient quantities, e.g. 2 onions for 1 meal + 1 onion for another = 3 onions
- Manually group the items roughly by location so I’m not zigzagging around the crowded supermarket in red-faced frustration.

### Automate the boring stuff

_So_ I made a thing which will do the tedious work for me 🎉 There’s a bit of upfront tedium in getting all the ingredients for each meal in a neat format but it’s only once per meal and when it’s in the spirit of saving time in the future, it can even be fun (uh, in my opinion).

This side project used the following tech:

- NodeJS (what else?)
- TypeScript (Ooh, type-safety, how novel)
- Serverless/AWS Lambda (It’s the future)
- The Google sheets API
- Some _sick_ google sheets formulas that I was very pleased with

I’ll go into more detail on how this went later on.

<h3 id="sharing-is-caring">Sharing is caring</h3>

Mate, you can use this too! If you want.

Here’s a generic meal spreadsheet I adapted from the one I’ve honed over a period of extended and regular use and have poured lots of love into. You can copy it and make your own:

[Generic meal spreadsheet](https://docs.google.com/spreadsheets/d/1jxZ8m7SKLJ4_YwjqKP6gg3QKQIGM7Ch9Bnv0Ttu5zkI/edit)

And here’s the codez, which you can deploy to your own Amazon Web Services account, if you’re so inclined:

[github.com/laurieboyes/sheets-shop](github.com/laurieboyes/sheets-shop)

### How does it work though?

So I have my meals spreadsheet. Every time I try out a new recipe, I add it to the list, along with a load of extra data.\*

\*_Juuust in case you’re interested, my extra data includes:_

- _Comments, such as ‘Nice but go easier on the szechuan pepper next time because Soph thought she was having a stroke’_
- _When we last had it (being able to sort by recency is good because it makes it easier to find things we've not had in a while)_
- _Whether we typically get any leftovers out of it_
- _Whether it's sufficiently low-effort to make on a weeknight_
- _Whether I can get all the ingredients from the Tesco Express nearby_
- _Whether it’s vegetarian (I’m not currently a vegetarian but it’s good to when I’m considering entertaining)_
- _Total time taken (for the rare occasion on which I remember to record it)_

I’ve added a new checkbox field to the spreadsheet so I can select which ones I want included in my shopping list:

![checkbox](/assets/img/yummy-spreadsheets/checkbox.png){: class="edgeless-image"}
How neat is that

On another sheet, I’ve entered my list of ingredients for each of my meals:

![ingredients](/assets/img/yummy-spreadsheets/ingredients.png){: class="edgeless-image"}

Cool.

And on yet another sheet (I did warn you about the upfront labour), I have a list of ingredients and their types:

![ingredient-types](/assets/img/yummy-spreadsheets/ingredient-types.png){: class="edgeless-image"}

Cool cool cool, ready to go.

When I’m getting ready to head out to the shops, I tick the checkboxes against each of my desired meals, and click the link to the API Gateway that’s in front of my shopping list generator lambda function.

The lambda function uses the Google Sheets API to ask my spreadsheet what up and what the ingredients are for the meals I want.

It then munges together all the ingredient quantities, with some degree of tolerance for different units (e.g. it’ll happily merge grams and kilograms), sorts them by their ‘types’ I also defined on the spreadsheet, and returns me a nice convenient list:

![finished-list](/assets/img/yummy-spreadsheets/finished-list.png){: class="edgeless-image"}

It may not be pretty visually, but a low-stress supermarket experience is a beautiful thing.

### Bits that were interesting (relatively)

I’ll spare you _all_ of the details, but certain parts of this side project were particularly fun to do.

#### Parsing the ingredients

I needed a way of entering ingredients that found a compromise between making them easy to add and making it easy for my list generator to understand them with its hyper-literal robot mind.

After a small amount of deliberation I settled on a system where the first token (by which I mean all of the characters before the first space) always defines the quantity:

<span class='highlight'>3cloves</span> garlic<br/>
<span class='highlight'>some</span> fresh ginger<br/>
<span class='highlight'>4</span> spring onions<br/>
<span class='highlight'>1tsp</span> szechuan peppercorns<br/>

This system gives the my code a leg up in understanding what’s going on, as it can easily pick out the text before the first space. It’s then a case of using regular expressions to pick out the number and, if there is one, the unit.

![ingredients-regex](/assets/img/yummy-spreadsheets/ingredients-regex.gif){: class="edgeless-image"}
I _love_ it when I find a good reason to use regular expressions (thanks [regex101.com](https://regex101.com/))

It’s also pretty easy to enter them with my human typing fingers, so it’s a win-win

#### Some _sick_ google sheets formulas (formulae?)

This project definitely stretched my spreadsheet formula skills and it was always with a mixture of pride and relief when I finally got each of these working.

Sick formula number 1: Retrieve the value of the checkbox on the meals sheet to copy it onto the ingredients sheet (for slightly easier Google Sheets API querying)

```
=VLOOKUP(G1, INDIRECT("The other spreadsheet!A:B"), 2)
```

![checkbox-copy-formula](/assets/img/yummy-spreadsheets/checkbox-copy-formula.gif){: class="edgeless-image"}

Sick formula number 2: Conditional formatting to indicate if a selected recipe doesn’t have an entry in the ingredients sheet:

```
=AND(B1 = TRUE,COUNTIF(INDIRECT("'Ingredients'!1:1"),A1) <= 0)
```

![no-ingreeds-formula](/assets/img/yummy-spreadsheets/no-ingreeds-formula.png){: class="edgeless-image"}
Red means _danger_.

Sick formula number 3: Conditional formatting to indicate when an ingredient doesn’t have a category defined on the Ingredient types sheet:

```
=AND(NOT(ISBLANK(H4)),COUNTIF(INDIRECT("'Ingredient types'!A:A"),RIGHT(H4,LEN(H4) - FIND(" ", H4))) <= 0)
```

![no-type-formula](/assets/img/yummy-spreadsheets/no-type-formula.png){: class="edgeless-image"}
The consequences of this are just that it won’t be grouped together with its collocated ingredients in the resulting list, so this orangey yellow colour will do for severity.

### Talking tech

Here are my thoughts, feelings, and motivations around the tech choices I made for this.

#### TypeScript

The first time I encountered TypeScript was in December 2016 when one of my favourite colleagues used it to write a new service as his final contribution before departing the company. When I subsequently came to maintaining it, I was overcome with frustration and disbelief at how difficult I was finding it to make my simple changes. He told me later that one day we’d all see the light, and it’s important to me that he never finds out that he was right and that these days I actually think it’s super useful.

The usefulness of type safety stole up on me gradually. Having made the decision a couple of years into my career to reject Java and all its mad verbosity for the free-and-easy alternative that was JavaScript, I had come to see static typing as a hangover from a less ✨agile ✨time.

But then more recently I found myself encountering bugs and thinking ‘dang, I should have written a unit test to check that’ or ‘dang, I should have put some defensive type checks in for those function parameters’, and then it occurred to me that _instead_ I could do what lots of other JavaScript fans are doing these days and write in TypeScript.

A turning point for me was when I heard that you can write TypeScript but still keep it loosey goosey by allowing for ‘implicit any’ (‘any’ as in ‘anything goes’) so you didn’t have to actually write any type definitions if you don’t want to, and you can add them later where you feel they’ll be most useful.

This is how I approached it in this project and it was great. I ended up writing just a few types but they were very helpful. I really like the fact that if I want to add a new property to a type, I can update an interface and it’ll immediately tell me everywhere in the project I need to update the code that uses it. It’s like really low-rent magic.

_(In the n months since I wrote this system/blog though, my TypeScript usage has evolved and I’ve done away with implicit any)_

#### Serverless framework + AWS Lambda

I absolutely love working with serverless stuff now. It just feels so easy to get stuff going.

A big part of this is [the Serverless framework](https://serverless.com/framework/docs/getting-started/), which is a properly good framework for deploying cloud functions and configuring event sources such as API Gateways and time schedules with super easy config. I’m not exaggerating. I love it.

AWS Lambda got even better in April 2018 (shortly before I wrote this system) when it started allowing you to run stuff in Node 8.10, opening the door to my new favourite syntactic sweeties, `async` and `await`, which made for some very neat promise-based asynchronous code.

#### The Google Sheets REST API

I’m glad I chose to go with the sheets API route. The alternative was to write a script in the Google Sheets script editor and dumping the results into another sheet. I have had a go at this sort of thing before but found it a bit of an uphill struggle, and it leaves no room for nice unit tests or TypeScript or whatever.

The API is fairly intuitive to use and very nicely documented (I’m sure if you’re interested you’ll google it but here are the Google Sheets REST API docs anyway).

### Bon appétit

In conclusion, this was a fun mini project and I’ll be making use of its fruits for many years to come. Hooray!

As I mentioned, if you wanted to try and integrate this approach into your own life, I’ve made my code and my spreadsheet template (with all those juicy formulas) available for all. Find them in the [Sharing is caring section](#sharing-is-caring).
