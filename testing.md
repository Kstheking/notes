# Testing 
#####  [Original Article](https://medium.com/welldone-software/an-overview-of-javascript-testing-7ce7298b9870)  

### Unit Testing 
Testing of individual units like functions or classes by supplying input and making sure the output is as expected
Complex dependencies and interactions to the outside world are stubbed or mocked.

### Integration tests 
Testing processes across several units to achieve their goals, including their side effects

### End to end tests
(also known as e2e tests or functional tests)- Testing how scenarios function on the product itself, by controlling the browser or the website. These tests usually ignore the internal structure of the application entirety and look at them from the eyes of the user like on a black box.

```
Go to page "https://localhost:3303"

Type "test-user" in the field "#username"

Type "test-pass" in the field "#password"

Click on "#login"

Expect Page Url to be https://localhost:3303/dashboard

Expect "#name" to be "test-name"
```

#### Smoke tests (aka sanity checks) 
A simple integration test where we just check that when the system under test is invoked it returns normally and does not blow up.

#### Regression tests 
A test that was written when a bug was fixed. It ensures that this specific bug will not occur again. The full name is "non-regression test". It can also be a test made prior to changing an application to make sure the application provides the same outcome.

#### Acceptance test 
Test that a feature or use case is correctly implemented. It is similar to an integration test, but with a focus on the use case to provide rather than on the components involved.

#### System test
Tests a system as a black box. Dependencies on other systems are often mocked or stubbed during the test (otherwise it would be more of an integration test).

#### Pre-flight check
Tests that are repeated in a production-like environment, to alleviate the 'builds on my machine' syndrome. Often this is realized by doing an acceptance or smoke test in a production like environment.

## Running tests

### In the browser 
by creating an HTML page with the test libraries and test files included as JS scripts.

### Headless browser 
which is way to launch browsers where they run without actually rendering on the screen

### Tests can also be executed in Node.js
by simply importing the test files and dependent libraries. jsdom is commonly used in Node.js to simulate a browser-like environment using pure JavaScript.

## Test tool types

### Test Launchers 
are used to launch your tests in the browser or Node.js with user config. (Karma, Jasmine, Jest, TestCafe, Cypress, webdriverio)

### Testing Structure providers 
help you arrange your tests in a readable and scalable way. (Mocha, Jasmine, Jest, Cucumber, TestCafe, Cypress)

### Assertion functions 
are used to check if a test returns what you expect it to return and if its’t it throws a clear exception. (Chai, Jasmine, Jest, Unexpected, TestCafe, Cypress)

### Generate and display test progress and summary.
(Mocha, Jasmine, Jest, Karma, TestCafe, Cypress)

### Mocks, spies, and stubs
to simulate tests scenarios, isolate the tested part of the software from other parts, and attach to processes to see they work as expected. (Sinon, Jasmine, enzyme, Jest, testdouble)

### Generate and compare snapshots
to make sure changes to data structures from previous test runs are intended by the user’s code changes. (Jest, Ava)

### Generate code coverage
reports of how much of your code is covered by tests. (Istanbul, Jest, Blanket)

### Browser Controllers 
simulate user actions for Functional Tests. (Nightwatch, Nightmare, Phantom, Puppeteer, TestCafe, Cypress)

### Visual Regression tools 
are used to compare your site to its previous versions visually by using image comparison techniques.
(Applitools, Percy, Wraith, WebdriverCSS)
