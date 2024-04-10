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

<!-- - Feature: Describes a software funcionality.
- Scenario: Describes a specific test case for a feature. -->
- GIVEN: Describe the initial context of the scenario.
- WHEN: Describes the action that is performed in the scenario.
- THEN: Describes the expected result of the action. 

## Example
Let's see what a use case implementation would look like using Gherkin in conjunction with Jest, a popular testing framework for TypeScript:

```ts title="create-restaurant.spec.ts"
    it(`
        GIVEN a restaurant data
        WHEN I call to the use case to create a restaurant
        THEN the restaurant should be created with the correct data
    `, () => {
        const restaurantReposistory: RestaurantRepositoryPort =
            new RestaurantMockRepository();
        const action = new CreateRestaurantUseCase(restaurantReposistory);

        // GIVEN
        const restaurantRequestData: CreateRestaurantDto = {
            name: "Restaurant 1",
            description: "Restaurant 1 description",
            email: "restaurant1@gmail.com",
            capacity: 10,
        };

        // WHEN
        const restaurantCreated = await action.run(restaurantRequestData);

        // THEN
        expect(restaurantCreated.propsCopy.name.value).toEqual("Restaurant 1");
        expect(restaurantCreated.propsCopy.description.value).toEqual(
            "Restaurant 1 description",
        );
        expect(restaurantCreated.propsCopy.email.value).toEqual(
            "restaurant1@gmail.com",
        );
    })
```

The example presented here illustrates how to write clear and concise test cases that describe the expected behaviour of an application. Let us look at each part of the use case in more detail:

**GIVEN:** In this section the initial conditions for the test scenario are established. Data is created for a restaurant by simulating the information provided by the user, such as restaurant name, description, email and maximum customer capacity.

**WHEN:** This is where the action being tested is executed. The use case is called to create a restaurant, passing the previously established restaurant data.

**THEN:** This part defines the expectations of the result of the action. It checks that the restaurant created has the correct data as specified in the "Given" section. It is expected that the name, description and email of the created restaurant match the data initially provided.

The `time.value-object.spec.ts` test is used to verify the functionality of the Time value object in validating invalid time values. 
```ts title="time.value-object.spec.ts"
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
The test consists of three parts:

1. **GIVEN:** In this section the value object `time` is defined with an invalid time value ("24:30").

2. **WHEN:** A function `timeCreation` is defined that tries to create a new `Time` object using the variable time.

3. **THEN:** The `toThrowError` function is used to verify that the timeCreation function throws an exception with the message "Hour is invalid".



## Benefits of using Gherkin:
- **Improves communication**: Gherkin facilitates communication between development and business teams, as both can understand the tests clearly and easily.

- **Increases software quality**: Gherkin testing helps detect bugs in the early stages of development, which reduces the time and cost to fix them.

- **Encourages iterative development**: Gherkin tests can be written and executed quickly and easily, which facilitates iterative software development.