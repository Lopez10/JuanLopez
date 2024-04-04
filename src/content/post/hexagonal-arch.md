---
title: "Introduction to hexagonal architecture"
description: "Discover how this architecture can improve the readability and maintainability of your projects."
publishDate: "28 March 2024"
tags: ["architecture"]
draft: true
---

In the realm of software development, the rapid pace of technological change can be overwhelming. Frameworks come and go, but business logic remains. Hexagonal architecture offers a solution by allowing us to abstract from the technological chaos. By separating the core logic of our application from the implementation details, we can easily adapt to new frameworks and technologies without rewriting everything from scratch. In this article, we'll explore how hexagonal architecture promotes technological abstraction and why you should care.

Imagine you are building a house. You want it to be solid, easy to maintain, and you want to be able to change the windows or the front door without having to rebuild everything from scratch. Well, that's basically what hexagonal architecture does for your software.

## Dependency Inversion
The hexagonal architecture promotes the principle of dependency inversion, where high-level dependencies (the business logic) do not depend on low-level concrete implementations (e.g., data storage technologies or user interface frameworks), but depend on abstractions. This allows concrete implementations to be easily interchanged without affecting the business logic.

## How does it work?
Instead of having your business logic tied to details like databases or user interfaces, the hex architecture puts it at the centre, surrounded by ports that define how your application communicates with the outside world. These ports have adapters that handle the connection to different technologies, such as databases, web services...

## Why is it Great?
* **Flexibility:** Switching databases or frontend frameworks shouldn't be a nightmare. With the hex architecture, you simply swap adapters and you're done.

* **Stability:** Separating business logic makes it easier to test your code. You can isolate it and write tests without worrying about implementation details.

* **Maintenance:** Because your code is well organised and decoupled, making changes or adding new functionality is much less painful. Goodbye to code clutter!

## Rules

## Example
Imagine we are building a system to manage restaurant reservations. Let's explore how we could design this system using the hexagonal architecture, layer by layer:
#### Domain
The domain layer encapsulates the business logic of our restaurant. It defines how tables are reserved, how orders are placed and processed... This is where the magic happens, where we translate real-world concepts into software code that drives our restaurant operations.

By keeping the domain layer focused and independent, we ensure that our business logic remains clear and maintainable. Changes to the restaurant's processes or requirements can be easily incorporated within this layer without affecting the rest of the system.

Let's take as an example a key entity in the reservation management system: the restaurant. In this entity we define what a restaurant is, including properties such as its name, capacity and, above all, its unique identifier. It is essential that each entity has a unique identifier to facilitate its identification and manipulation within the system. In our article [link to article], we explain in detail the differences between a value object and an entity. Within the entity are also defined the actions that can be performed with it.
```ts title="Restaurant.entity.ts"
interface RestaurantProps {
    id: Id;
    name: string;
    capacity: number;
}

class Restaurant {
    constructor(private props: RestaurantProps) {
        this.props = props;
    }
    updateCapacity(capacity: number) {
        this.props.capacity = capacity;
    }

    get capacity() {
        return this.props.capacity;
    }

    isGreaterThanCapacity(capacity: number) {
        return this.props.capacity > capacity;
    }
}
```
This entity encapsulates the information and behaviour related to a restaurant. It is independent of any specific technology or implementation, which makes it easy to understand and maintain over time.



At the domain layer, we also define the ports that allow us to interact with the outside world. A common port in many systems is the repository port, which defines how information in the database is accessed and manipulated.

```ts title="Restaurant.repository.port.ts"
interface RestaurantRepositoryPort {
    findRestaurantByName(name: Name): Promise<Restaurant | null>;
    updateRestaurant(restaurant: Restaurant): Promise<void>
}
```

This port defines a set of operations that can be performed on restaurants, such as getting a restaurant by its identifier, updating the restaurant... The concrete implementations of this port, such as a SQL database or an in-memory repository, are located in the infrastructure layer and are connected to the domain layer through interfaces such as this one.

In short, the domain layer defines the business rules and key entities of our reservation management system, such as the restaurant, and establishes the ports that allow us to interact with the outside world, such as the repository port. It is the core of our system, where the real essence of what makes our system work resides.

#### Application
```ts title="retrieve-restaurant.use-case.ts"
console.log("Title example");
```

#### Infrastructure
```ts title="Restaurant.postgre.repository.ts"
console.log("Title example");
```

#### Presentation
```ts title="Restaurant.controller.ts"
console.log("Title example");
```