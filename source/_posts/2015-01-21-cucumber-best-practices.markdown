---
layout: post
title: "Cucumber Best Practices"
date: 2015-01-21 09:14:44 +1300
comments: true
categories: [ruby,cucumber]
---
[cucumber](http://cukes.info/) is a BDD(Behaviour driver development) framework. Basically test cases are written in plain text which is called Gherkin language, just in Given-When-Then steps. After that, developer will write implementation for those steps.

It's good to bridge the communication between business requirement and implementation. The ideal work flow would be business analysis or QA write features and developer implements them. However, in a real world, developer write both of them. Cucumber is different programming model than what we normmaly use like Java, ruby, etc. It is not mature enough like OO which has tons of patterns and best practices to apply.

Below are some best practices when I use cucumber for one project.

## Write declarative features
Scenarios should be written like a user would describe them.

Beware of scenarios that only describe clicking links and filling in form fields, or of steps that contain code or CSS selectors. This is just another variant of programming, but certainly not a feature description.


**Bad**

```Cucumber
Scenario: Adding a todo item
    Given I have a todo list named "Mondays list"
    When I go to the home page
    And I fill in "username" with "dave"
    And I fill in "password" with "secret"
    And I press "Log In"
    And I go to the todo page
    And I click on link "Mondays list"
    And I fill in "todo" with "Grab some milk"
    And I press "Add todo"
    Then I should see "Todo item added successfully"
```

**Good**
```Cucumber
Scenario: Adding a todo item
    Given I have a todo list
    And I am logged in as a normal user
    When I add a todo item
    Then It should be added to the todo list
```

## Use `As a <role>, I want <goal/desire> So that <benefit>` format for feature description
Describe feature in user story style. Starts the feature and gives it a title, then follow with user story format.

**Bad**
```Cucumber
Feature: Create an account

  Scenario: ...
```

**Good**
```Cucumber
Feature: Create an account
  As a user
  I want to create an account for me
  So that I can re-login to the same account

  Scenario: ...
```

## Use `should` in each `Then` and following `And` steps
The purpose of `Then` steps is to observe outcomes and verify result. Using `should` word as a convention makes it easy to understand, and make sure do verification in implementation step.

**Bad**
```Cucumber
Then My job is displayed in the table
```

**Good*
```Cucumber
Then My job should be displayed in the table
```

## Capitalize first letter of every step
Each step is independant, capitalize first letter makes a nice format.

**Bad**
```Cucumber
Then my job should be displayed in the table
```

**Good**
```Cucumber
Then My job should be displayed in the table
```


## Use page object model
Don't write implementation in step definition, use page object model.

**Bad**

```ruby
When(/^I search for my job$/) do
  visit path_to('manage jobs')
  find('a', :text => 'Filter Jobs').click
  fill_in('users-id-search', :with=>"")
  page.find('#users-id-search').native.send_keys(:backspace)
  fill_in('requisition.title', :with => @job.title)
  click_button('Search')
end
```

**Good**

_job_steps.rb_
```ruby
When(/^I search for my job$/) do
  @app.manage_job_page.load
  @app.manage_job_page.search @job.title
end
```
_manage_job_page.rb_
```ruby
def search(job_title)
  find('a', :text => 'Filter Jobs').click
  fill_in('users-id-search', :with=>"")
  page.find('#users-id-search').native.send_keys(:backspace)
  fill_in('requisition.title', :with => @job.title)
  click_button('Search')
end
```

## Use background
If all the scenario in one feature file have the same steps, put them in the background.

**Bad**
```Cucumber
Scenario: Foo
  Given I am logged in as an admin
  And ....

Scenario: Bar
  Given I am logged in as an admin
  And ....
```

**Good**
```Cucumber
Background:
  Given I am logged in as an admin

Scenario: Foo
  Given ....

Scenario: Bar
  Given ....
```


## Use Tags
Tags are a great way to organise your features and scenarios.

**Good**
```Cucumber
@job @smoke
Feature: Job management
```

## Avoid to use step params
Step params is a smell which the feature is not declarative.

The parameter in feature should **not** be used as **implementation parameter**.

**Bad**
```Cucumber
Given I complete registration from using email "test@example.com"
```

**Good**
```Cucumber
Given I complete registration form using a valid email
```


## Avoid to use scenario outlines
A Scenario Outline provides a parametrized scenario script. However, it easily become an anti-pattern if you use it too many.

Each scenario with example actually means a different scenario. It's better to **use scenario with a meaningful name** instead of using scenario example.

Scenario outlines uses step params, which should avoid to use.

Beware of feature that only has one scenario but have a long list of examples. It could be a smell that too many different scenario are put together.

## Avoid to use data tables
Data tables provides data for implementation. It is against the rule of feature should be declarative and business focus.

Feature should not provide any detail implementation or data for testing.


## Use Capybara `find` whenever possible
`find` will wait for a set amount of time and continuously retry finding the element until either the element is found or the time expires.

**Bad**

```ruby
first(".active").click
```

**Good**
```ruby
find(".active", match: :first).click
```
