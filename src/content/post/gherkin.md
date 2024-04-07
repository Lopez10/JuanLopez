---
title: "Gherkin"
description: "Helps to clearly define scenarios and test cases, facilitating communication in agile development and BDD teams."
publishDate: "28 March 2024"
tags: ["test"]
draft: true
---

Gherkin is a Domain Specific Language (DSL) used to write acceptance tests and behavioral specifications for software. It is a simple and easy-to-read language for both developers and non-developers, making it an ideal tool for communication between development and business teams.

## What is Gherkin used for?

Gherkin is mainly used in the context of behavior-driven development (BDD), a software development methodology that focuses on software behavior from the user's perspective.

## How does Gherkin work?

Normally Gherkin is used through third party tools, but we are going to make use of gherkin through test documentation. 

There are a number of keywords to make use of gherkin:

- Feature: Describes a software funcionality.
- Scenario: Describes a specific test case for a feature.
- GIVEN: Describe the initial context of the scenario.
- WHEN: Describes the action that is performed in the scenario.
- THEN: Describes the expected result of the action. 

## Example
```ts title="create-restaurant.spec.ts"
    it(`
        GIVEN an invalid restaurant data
        WHEN I create a new restaurant with this data 
        THEN an error message is thrown 
    `, () => {
        // GIVEN
        // WHEN
        // THEN
    })
```

```ts title="dni.value-object.spec.ts"
    it(`
        GIVEN a string with 8 numbers
        AND a letter that does not match
        WHEN I test if the letter is valid
        THEN the method returns false
    `, () => {
        const isLetterValid = Dni.isLetterValid(12345678, 'X');
        expect(isLetterValid).toBe(false);
    });
```

```ts title="dni.value-object.spec.ts"
    it(`
        GIVEN a invalid hour time data
        WHEN I create a new Time
        THEN the Time value object is not created
        AND an error is thrown
    `, () => {
        // GIVEN
        const time = '24:30';

        // WHEN
        const timeCreation = () => new Time(time);

        // THEN
        expect(timeCreation).toThrowError('Hour is invalid');
    });
```



## Benefits of using Gherkin:
- **Improves communication**: Gherkin facilitates communication between development and business teams, as both can understand the tests clearly and easily.
- **Increases software quality**: Gherkin testing helps detect bugs in the early stages of development, which reduces the time and cost to fix them.
- **Encourages iterative development**: Gherkin tests can be written and executed quickly and easily, which facilitates iterative software development.