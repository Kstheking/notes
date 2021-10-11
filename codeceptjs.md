# CodeceptJs

CodeceptJS is a modern end to end testing framework with a special BDD-style syntax. The tests are written as a linear scenario of the user's action on a site.

```
Feature('CodeceptJS demo');

Scenario('check Welcome page on site', ({ I }) => {
  I.amOnPage('/');
  I.see('Welcome');
});

```

Tests are expected to be written in ECMAScript 7.

Each test is described inside a Scenario function with the I object passed into it. The I object is an actor, an abstraction for a testing user. The I is a proxy object for currently enabled Helpers.

## Setup 

### install 
`npm install codeceptjs`

### initialize 
`codeceptjs init`

### run tests
`npm run codeceptjs`  

#### Specify paths (can also point to features) 
`npx codeceptjs run --steps features/*`  (steps for printing steps)

#### run only features 
`npx codeceptjs run --features`  

#### run only tests 
`npx codeceptjs run --tests`

#### run tagged features or scenarios 
###### (you can tag scenarios as well as features )

`npx codeceptjs run --grep "@tag_name"`

### Writing tests 

```
Feature('random');

Scenario('test something', ({ I }) => {
    I.amOnPage('https://github.com');
    I.see("Why GitHub?"); 
});

```

## [Commands](https://codecept.io/commands/)

## Using Gherkin

### initializing gherkin 
`npx codeceptjs gherkin:init`

### Defining steps after writing features
`npx codeceptjs gherkin:snippets`  
`npx codeceptjs gherkin:snippets [--path=PATH] [--feature=PATH]` for additional details

## Test writing 

### see a text 
`I.see("Text");`

### click something 
`I.click("Something");`

### See an element 
`I.seeElement(".clas");`

### see if current url equal 
`I.seeCurrentUrlEquals("/login");`

### headless 
getting started  > configuration > figure it out 

## Locators 

`node[attribute_name = ‘attribute_value’]`  
- node is the tag name of the HTML element, which needs to locate.
- attribute_name is the name of the attribute which can locate the element.
- attribute_value is the value of the attribute, which can locate the element.

`input[id=firstName]`  

`input#firstName`  

`textarea.form-control`  

`textarea[placeholder=Current Address]`  

`textarea#currentAddress[placeholder=Current Address]`  

### parent child 
suppose div is the parent then 
`div>textarea[placeholder=Current Address]`    

select 2nd child of select menu 
`select#oldSelectMenu>option:nth-of-type(2)`  

### locate using starting text 
`input[id^=userN]` 

### locate using ending text 
`input[id$=ame]` 

### locate if contains text 
`input[id*=erNa]` 
    



