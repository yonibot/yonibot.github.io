---
layout: post
title: "Testing with Selenium"
date: 2013-07-06 17:09
comments: true
categories: 
---
When doing feature testing with the Capybara gem, you use **racktest**as
the default testing driver. Racktest is fast, as it's a headless driver
- it simulates the browser experience without firing up an actual
browser. But if you need javascript, you need to use either
**Selenium**, which actually fires up Firefox and simulates the entire
user flow before your eyes, or **Capybara WebKit**, another headless
driver that is faster than Selenium, but supports js. Once you've
configured Capybara, invoking selenium just requires that you set "js:
true" explicitly after a feature / describe line (depending on whether
you decide to use Capybara's DSL or the regular Rspec syntax). Example:
`feature 'User interacts with the queue', js: true do` For this to work,
you need to disable transactional\_fixtures, a feature offered by rspec
that opens a database transaction at the beginning of a test, and then
rolls it back at the end. This feature [does not work with Capybara
javascript tests][], so disable it by placing
`self.use_transactional_fixtures = false` after the feature/describe
line (again, capybara dsl vs. rspec distinction) and before the
scenario/it line. If you want the firefox browser window to stop so that
you can play with one of the pages, you can drop a \<code\>sleep [x
seconds]\</code\> command anywhere in the test and the browser will wait
for the specified time before continuing on its way (and then closing if
it's the end of the test).

  [does not work with Capybara javascript tests]: http://weilu.github.io/blog/2012/11/10/conditionally-switching-off-transactional-fixtures/