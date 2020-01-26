---
layout: "post"
title: "This is instruction of jekyll"
date: 2020-01-22 20:48:59 -0700
author : 'mingi hong'
---

## Instruction of jekyll

# Installing jekyll

{% highlight ruby %}
sudo gem install jekyll
{% endhighlight %}

![jekyll1](/minglab/assets/jekyll1.png)
![jekyll2](/minglab/assets/jekyll2.png)

{% highlight ruby %}
jekyll new mywebsite
sudo gem install jekyll bundler
{% endhighlight %}

![jekyll3](/minglab/assets/jekyll3.png)

{% highlight ruby %}
bundle exec jekyll serve
{% endhighlight %}

![jekyll4](/minglab/assets/jekyll4.png)

`go to localhost:4000 to see your website`

Jekyll requires blog post files to be named according to the following format:

`YEAR-MONTH-DAY-title.MARKUP`

Where `YEAR` is a four-digit number, `MONTH` and `DAY` are both two-digit numbers, and `MARKUP` is the file extension representing the format used in the file. After that, include the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll.


# Connect with Git Hub Page(Free Hosting)

![gh-pages](/minglab/assets/gh-pages.png)

{% highlight ruby %}
git init
git checkout -b gh-pages
git status
git add .
git commit -m "initial commit"
git remote add origin "my repository url"
git push origin gh-pages
{% endhighlight %}

[jekyll-docs]: https://jekyllrb.com/docs/home