# Gherkin

Gherkin uses a set of special keywords to give structure and meaning to executable specifications. Each keyword is translated to many spoken languages

```
Feature: Guess the word

  # The first example has two steps
  Scenario: Maker starts a game
    When the Maker starts a game
    Then the Maker waits for a Breaker to join

  # The second example has three steps
  Scenario: Breaker joins a game
    Given the Maker has started a game with the word "silky"
    When the Breaker joins the Maker's game
    Then the Breaker must guess a word with 5 characters
```

## Keywords 
Each line that isn’t a blank line has to start with a Gherkin keyword, followed by any text you like. The only exceptions are the feature and scenario descriptions.

### Primary Keywords 
-  `Feature`
-  `Rule`
-  `Example`
-  `Given`, `When`, `Then`, `And`, `But` for steps (or `*`)
-  `Background` 
-  `Scenario Outline`  (or `Scenario Template`)
-  `Examples` (or `Scenarios`)

### Secondary Keywords 
-  `"""` (Doc Strings)
-  `|` (Data tables) 
-  `@` (Tags) 
-  `#` (Comments)

#### Feature 
The purpose of the Feature keyword is to provide a high-level description of a software feature, and to group related scenarios.

```
Feature: Guess the word

  The word guess game is a turn-based game for two players.
  The Maker makes a word for the Breaker to guess. The game
  is over when the Breaker guesses the Maker's word.

  Example: Maker starts a game
```

The free format description for Feature ends when you start a line with the keyword Background, Rule, Example or Scenario Outline (or their alias keywords).
You can place [tags](https://cucumber.io/docs/cucumber/api/#tags) above Feature to group related features, independent of your file and directory structure.

#### Descriptions
 like Feature can also be placed underneath Example/Scenario, Background, Scenario Outline and Rule.
 Descriptions can be in the form of Markdown 

#### Rule
 The purpose of the Rule keyword is to represent one business rule that should be implemented. It provides additional information for a feature.A Rule is used to group together several scenarios that belong to this business rule. A Rule should contain one or more scenarios that illustrate the particular rule.

 ```
 # -- FILE: features/gherkin.rule_example.feature
Feature: Highlander

  Rule: There can be only One

    Example: Only One -- More than one alive
      Given there are 3 ninjas
      And there are more than one ninja alive
      When 2 ninjas meet, they will fight
      Then one ninja dies (but not me)
      And there is one ninja less alive

    Example: Only One -- One alive
      Given there is only 1 ninja alive
      Then he (or she) will live forever ;-)

  Rule: There can be Two (in some cases)

    Example: Two -- Dead and Reborn as Phoenix
      ...

 ```

#### Example
 The keyword Scenario is a synonym of the keyword Example.
 In addition to being a specification and documentation, an example is also a test.

#### Steps   
 Keywords are not taken into account when looking for a step definition. This means you cannot have a Given, When, Then, And or But step with the same text as another step.

 ```
 Given there is money in my account
Then there is money in my account
 ```

 These steps are duplicates
 This is done to reduce ambiguity 

#### Given   
 Given steps are used to describe the initial context of the system - the scene of the scenario.
The purpose of Given steps is to put the system in a known state before the user (or external system) starts interacting with the system (in the When steps). 
It’s okay to have several Given steps (use And or But for number 2 and upwards to make it more readable).

#### When   
When steps are used to describe an event, or an action. This can be a person interacting with the system, or it can be an event triggered by another system.

#### Then  
Then steps are used to describe an expected outcome, or result.

The step definition of a Then step should use an assertion to compare the actual outcome (what the system actually does) to the expected outcome (what the step says the system is supposed to do).

#### *
Gherkin also supports using an asterisk (*) in place of any of the normal step keywords.

```
Scenario: All done
  Given I am out shopping
  And I have eggs
  And I have milk
  And I have butter
  When I check my list
  Then I don't need anything
  ```

  to 

  ```
  Scenario: All done
  Given I am out shopping
  * I have eggs
  * I have milk
  * I have butter
  When I check my list
  Then I don't need anything
  ```
  
#### Background 
you’ll find yourself repeating the same Given steps in all of the scenarios in a Feature.
You can literally move such Given steps to the background, by grouping them under a Background section.
A Background allows you to add some context to the scenarios that follow it. It can contain one or more Given steps, which are run before each scenario, but after any [Before hooks](https://cucumber.io/docs/cucumber/api/#hooks).

A Background is placed before the first Scenario/Example, at the same level of indentation.

```
Feature: Multiple site support
  Only blog owners can post to a blog, except administrators,
  who can post to all blogs.

  Background:
    Given a global administrator named "Greg"
    And a blog named "Greg's anti-tax rants"
    And a customer named "Dr. Bill"
    And a blog named "Expensive Therapy" owned by "Dr. Bill"

  Scenario: Dr. Bill posts to his own blog
    Given I am logged in as Dr. Bill
    When I try to post to "Expensive Therapy"
    Then I should see "Your article was published."

  Scenario: Dr. Bill tries to post to somebody else's blog, and fails
    Given I am logged in as Dr. Bill
    When I try to post to "Greg's anti-tax rants"
    Then I should see "Hey! That's not your blog!"

  Scenario: Greg posts to a client's blog
    Given I am logged in as Greg
    When I try to post to "Expensive Therapy"
    Then I should see "Your article was published."
```

Background is also supported at the Rule level

```
Feature: Overdue tasks
  Let users know when tasks are overdue, even when using other
  features of the app

  Rule: Users are notified about overdue tasks on first use of the day
    Background:
      Given I have overdue tasks

    Example: First use of the day
      Given I last used the app yesterday
      When I use the app
      Then I am notified about overdue tasks

    Example: Already used today
      Given I last used the app earlier today
      When I use the app
      Then I am not notified about overdue tasks
  ...
  ```

#### Scenario Outline 
The Scenario Outline keyword can be used to run the same Scenario multiple times, with different combinations of values.
The keyword Scenario Template is a synonym of the keyword Scenario Outline.

```
Scenario: eat 5 out of 12
  Given there are 12 cucumbers
  When I eat 5 cucumbers
  Then I should have 7 cucumbers

Scenario: eat 5 out of 20
  Given there are 20 cucumbers
  When I eat 5 cucumbers
  Then I should have 15 cucumbers
  ```

to 

```
Scenario Outline: eating
  Given there are <start> cucumbers
  When I eat <eat> cucumbers
  Then I should have <left> cucumbers

  Examples:
    | start | eat | left |
    |    12 |   5 |    7 |
    |    20 |   5 |   15 |
```

A Scenario Outline must contain an Examples (or Scenarios) section.

#### Step Arguments 
In some cases you might want to pass more data to a step than fits on a single line. For this purpose Gherkin has Doc Strings and Data Tables.

##### Doc Strings

```
Given a blog post named "Random" with Markdown body
  """
  Some Title, Eh?
  ===============
  Here is the first paragraph of my blog post. Lorem ipsum dolor sit amet,
  consectetur adipiscing elit.
  """
```

In your step definition, there’s no need to find this text and match it in your pattern. It will automatically be passed as the last argument in the step definition.

#####  Data Tables 
Data Tables are handy for passing a list of values to a step definition:

```
Given the following users exist:
  | name   | email              | twitter         |
  | Aslak  | aslak@cucumber.io  | @aslak_hellesoy |
  | Julien | julien@cucumber.io | @jbpros         |
  | Matt   | matt@cucumber.io   | @mattwynne      |
```

Just like Doc Strings, Data Tables will be passed to the step definition as the last argument.  
