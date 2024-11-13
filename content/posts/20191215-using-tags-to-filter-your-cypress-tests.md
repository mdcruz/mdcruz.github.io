---
author: "Marie Cruz"
title: "Using tags to filter your Cypress tests"
date: "2019-12-15"
tags:
- testing
- tools
- test-automation
---

This week, I have been helping my colleagues migrate our existing WebdriverIO tests to Cypress. For anyone who wants to know what Cypress is and how it's different from Selenium, check out this [webinar](https://applitools.com/blog/cypress-vs-selenium) from Gil Tayar. Anyway, while migrating some of the tests, I remembered that the existing test suite was using Cucumber tags to filter which tests are run on which test environments. Test filtering is currently a useful feature for us as we only run a subset of tests on PR environments. With the Cypress framework that we currently have, we have decided to follow the standard approach of writing the tests using Mocha as its test runner as opposed to Cucumber. While Mocha has a feature to filter its tests by using the —grep pattern, this is currently not supported within Cypress and have been requested as a new feature by their users. I didn’t really want to use the Cypress Cucumber plugin just to utilise the tagging feature so I decided to implement a simple workaround which I am planning to propose to my colleagues while I wait for Cypress to implement this feature.

The first thing that I did was I created a module called test-filter.ts which basically filters your Cypress tests based on what tag or tags are provided. This accepts two parameters, the tags that you defined on your spec file and the function that will run when the tags are found. If the environment variable *TEST_TAGS* exist, we simply split its values and store this to an array variable called tags. The `Array.some()` function is then used which would return true if there is at least one tag from the definedTags array that is included in the tags array. If it's true, we run the test.

```
/// <reference types="Cypress" />

const TestFilter = (definedTags: string[], runTest: Function) => {
  if (Cypress.env('TEST_TAGS')) {
    const tags = Cypress.env('TEST_TAGS').split(',');
    const isFound = definedTags.some(definedTag => tags.includes(definedTag));

    if (isFound) {
      runTest();
    }
  }
};

export default TestFilter;
```

The next step is to import this module to my spec files. As an example, the below code snippet shows that I have added in two tags for this test (smoke and test). The function that I pass to `TestFilter` is the entire describe block containing my one test. 

```
/// <reference types="Cypress" />

import TestFilter from '../../test-filter';

TestFilter(['smoke', 'test'], () => {
  describe('Taboola', () => {
    beforeEach(() => {
      cy.viewport('macbook-13');
    });

    it('should exist on an article page', () => {
      cy.visit(Cypress.env('TEST_ARTICLE'));
      cy.waitForAdRequest();

      cy.get('div[data-mode="Feeder"]').should('exist');
      cy.get('div[data-mode="alternating-thumbnails-a"]').should('exist');
    });
  });
});
```

To run the tests with the tagging functionality, I had to export an environment variable to Cypress called *TEST_TAGS*. For example, if we have 4 spec files and 3 of the spec files are tagged 'regression' and 1 is tagged 'smoke', running the command below will only execute the test tagged with 'smoke'.

`CYPRESS_TEST_TAGS=smoke npm run cy:run:local:dev`

<figure>
  <img src="../../images/cypress_tags.png" alt="Test output with tags">
  <figcaption>The test tagged with 'smoke' is executed while the other tests are not</figcaption>
</figure>

Multiple tags are also supported and this can be run by running the example command below:Multiple tags are also supported and this can be run by running the example command below:

`CYPRESS_TEST_TAGS=testA,testB npm run cy:run:local:dev`

There is a limitation though with this approach and it's not as powerful as Cucumber tags. For example if we look at the code snippet below and we only want to run 'Test B', running the command `CYPRESS_TEST_TAGS=testB npm run cy:run:local:dev`  will still run 'Test A' since it's not wrapped with TestFilter. Since I don’t want to tag 'Test A' in this example, for it to work properly, I had to also wrapped this test with TestFilter and pass it an empty array.

```

/// <reference types="Cypress" />

import TestFilter from '../../test-filter';

describe('My Test with tags', () => {
  it('Test A', () => console.log('test A'));

  TestFilter(['testB'], () => it('Test B', () => console.log('test B')));
});
```

Although it has a limitation, using this approach clearly has some advantages that I think our team can benefit at. If we need to only run, let's say, our smoke tests, we don't need to define a separate folder for it in Cypress and write duplicate tests. We can just pick a selection of our existing tests and tag it accordingly.


To see an example project in action, please look at my [GitHub repo](https://github.com/mdcruz/cypress-test-tags) for reference.