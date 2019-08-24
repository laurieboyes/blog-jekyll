# My blog

[blog.lrnk.co.uk](http://blog.lrnk.co.uk/)

Made with [Jekyll](https://jekyllrb.com) and [Type Theme](https://github.com/rohanchandra/type-theme).

## Installing dependencies

You want to be on ruby version 2.4: https://stackoverflow.com/a/38194139

And also you probably want to do this

```
sudo chown -R $USER /Library/Ruby/Gems/
```

Definitely do these

```
gem install jekyll
gem install bundler -v 1.17.3 # Or whatever is the latest version of bundler 1
bundle _1.17.3_ install
```

## Running locally

```
jekyll serve
```

## Deploying

```
jekyll build
cp -r _site/* ../laurieboyes.github.io
```
