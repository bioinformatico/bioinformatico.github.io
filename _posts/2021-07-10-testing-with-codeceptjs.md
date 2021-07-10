---
layout: post
title:  "Testing with Codeceptjs"
date:   2021-07-10 9:10:00 -0500
categories: cypress
---

<div class="post-image" style="text-align: center;">
  <img src="{{site.baseurl}}/assets/img/codeceptjs-img.png">
</div>
<br/>

Codeceptjs is an end-to-end BDD framework aiming at writing readable tests with the user's perspective. It can be run along with Playwright, WebDriver, Puppeteer, TestCafe, Protractor, and Appium without changing the code.

In order to start a project we run this script:

{% highlight bash %}
npx create-codeceptjs .
{% endhighlight %}

Then we start our project with this command:

{% highlight bash %}
npx codeceptjs init
{% endhighlight %}

There is going to be a menu to configure the environment. Let's say we chose Playwright. Then we can create a folder called tests and start with the first test:

{% highlight javascript %}
Feature('My First Test');

Scenario('test add todo', ({ I }) => {
  I.amOnPage('https://todomvc.com/examples/react/#/');
  I.fillField('input[placeholder="What needs to be done"]', 'Cook soup');
  //I.fillField('user[email]','miles@davis.com');
  I.pressKey('Enter');
  within('.todo-list', () => {
    I.see('Cook soup')
  });
});
{% endhighlight %}

<div class="post-image" style="text-align: center;">
  <img src="{{site.baseurl}}/assets/img/todo-img.png">
</div>
<br/>

An these are the results on the command line:
<div class="post-image" style="text-align: center;">
  <img src="{{site.baseurl}}/assets/img/codecept-pass-test.png">
</div>
<br/>

{% highlight bash %}
{% endhighlight %}

{% highlight bash %}
{% endhighlight %}

{% highlight bash %}
{% endhighlight %}

