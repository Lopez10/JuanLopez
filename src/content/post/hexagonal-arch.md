---
title: "Introduction to hexagonal architecture"
description: "Discover how this architecture can improve the readability and maintainability of your projects."
publishDate: "28 March 2024"
tags: ["architecture"]
draft: true
---

In the realm of software development, the rapid pace of technological change can be overwhelming. Frameworks come and go, but business logic remains. Hexagonal architecture offers a solution by allowing us to abstract from the technological chaos. By separating the core logic of our application from the implementation details, we can easily adapt to new frameworks and technologies without rewriting everything from scratch. In this article, we'll explore how hexagonal architecture promotes technological abstraction and why you should care.

Imagine you are building a house. You want it to be solid, easy to maintain, and you want to be able to change the windows or the front door without having to rebuild everything from scratch. Well, that's basically what hexagonal architecture does for your software.

## How does it work?
Instead of having your business logic tied to details like databases or user interfaces, the hex architecture puts it at the centre, surrounded by ports that define how your application communicates with the outside world. These ports have adapters that handle the connection to different technologies, such as databases, web services...

## Why is it Great?
* **Flexibility:** Switching databases or frontend frameworks shouldn't be a nightmare. With the hex architecture, you simply swap adapters and you're done.

* **Stability:** Separating business logic makes it easier to test your code. You can isolate it and write tests without worrying about implementation details.

* **Maintenance:** Because your code is well organised and decoupled, making changes or adding new functionality is much less painful. Goodbye to code clutter!

## Example
Imagine we are building a system to manage restaurant reservations. Let's explore how we could design this system using the hexagonal architecture, layer by layer:
#### Domain
The domain layer encapsulates the business logic of our restaurant. It defines how tables are reserved, how orders are placed and processed... This is where the magic happens, where we translate real-world concepts into software code that drives our restaurant operations.

By keeping the domain layer focused and independent, we ensure that our business logic remains clear and maintainable. Changes to the restaurant's processes or requirements can be easily incorporated within this layer without affecting the rest of the system.

```ts title="Restaurant.entity.ts"
console.log("Title example");
```

```ts title="Restaurant.repository.port.ts"
console.log("Title example");
```

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