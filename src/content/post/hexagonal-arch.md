---
title: "Introduction to hexagonal architecture"
description: "Discover how this architecture can improve the readability and maintainability of your projects."
publishDate: "6 April 2024"
tags: ["architecture"]
draft: false
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
1. **Outside-In Dependency Rule:**

This rule states that dependencies between system layers should flow from the outside to the inside.

It means that internal layers (such as business logic) should not depend on external layers (such as infrastructure).

This promotes greater modularity and facilitates testing and component replacement.
 
2. **Dependency Inversion**

Dependency inversion is a key principle in the hexagonal architecture that promotes flexibility and modularity.

It refers to the practice of reversing control of dependencies from one component to another external component.

This is achieved through the use of interfaces and dependency injection, allowing concrete implementations to be easily interchanged without modifying the source code.

It facilitates the design of independent and reusable components, leading to cleaner, maintainable and scalable code.
 
3. **Separation of Concerns:**

This principle refers to the practice of dividing a system into components that handle a single responsibility or concern. In the hexagonal architecture, each layer of the system should be dedicated to a specific concern, such as business logic, presentation or data persistence.

4. **Single Responsibility Principle (SRP):** 

This principle states that a class or module should have only one reason to change. In the context of the hexagonal architecture, each component should be responsible for only one part of the system and should not have multiple responsibilities.

5. **Principle of Abstraction:**

This principle promotes the use of abstractions, such as interfaces, rather than concrete implementations in system design. Abstractions facilitate component exchange and unit testing by decoupling system components.

## Example
Imagine we are building a system to manage restaurant reservations. Let's explore how we could design this system using the hexagonal architecture, layer by layer:

#### Domain
The domain layer encapsulates the business logic of our restaurant. It defines how tables are reserved, how orders are placed and processed... This is where the magic happens, where we translate real-world concepts into software code that drives our restaurant operations.

By keeping the domain layer focused and independent, we ensure that our business logic remains clear and maintainable. Changes to the restaurant's processes or requirements can be easily incorporated within this layer without affecting the rest of the system.

Let's take as an example a key entity in the reservation management system: the restaurant. 

In this entity we define what a restaurant is, including properties such as its name, capacity and, above all, its unique identifier. 

It is essential that each entity has a unique identifier to facilitate its identification and manipulation within the system. 

In our article [link to article], we explain in detail the differences between a value object and an entity. Within the entity are also defined the actions that can be performed with it.

```ts title="Restaurant.entity.ts"
export interface RestaurantProps {
    id: Id;
    name: string;
    capacity: number;
}

export class Restaurant {
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
export interface RestaurantRepositoryPort {
    findRestaurantByName(name: Name): Promise<Restaurant | null>;
    updateRestaurant(restaurant: Restaurant): Promise<void>
}
```

This port defines a set of operations that can be performed on restaurants, such as getting a restaurant by its identifier, updating the restaurant... The concrete implementations of this port, such as a SQL database or an in-memory repository, are located in the infrastructure layer and are connected to the domain layer through interfaces such as this one.

In summary, the domain layer defines the business rules and key entities of our reservation management system, such as the restaurant, and establishes the ports that allow us to interact with the outside world, such as the repository port. It is the core of our system, where the real essence of what makes our system work resides.

#### Application
In the hexagonal architecture, the application layer is like the engine that drives all system operations. This is where the key business logic that defines the behaviour of our application is executed. To better understand how this layer works, we will explore a common use case: 
```ts title="retrieve-restaurant.use-case.ts"
export class RetrieveRestaurantUseCase
	implements UseCase<string, RestaurantDto[]>
{
	constructor(private readonly repository: RestaurantRepositoryPort) {}

	async run(
		id: string,
	): Promise<RestaurantDto[]> {
        const restaurantId = new Id(id)
		const restaurantFound = await this.repository.findOneById(restaurantId);

        if (!restaurantFound) throw new Error('Restaurant not found')

		return RestaurantMapper.toDto(restaurantFound);
	}
}
```
The `retrieve-restaurant` **use case** involves obtaining detailed information about a specific restaurant. This may include details such as the name of the restaurant, its address, opening hours, menu and any other relevant information.

When a request for `retrieve-restaurant` is received, the use case checks the parameters provided, such as the `ID` of the requested restaurant.

It then communicates with the domain layer to access the relevant domain objects, such as the restaurant repository.

Using the restaurant repository, the application layer retrieves the corresponding restaurant data from the database or any other data storage system.

Once the restaurant details are retrieved, the application layer formats them appropriately, using data transfer objects (DTOs), and returns them as a response to the client.

#### Infrastructure
The infrastructure layer is the component in charge of handling specific technical and implementation details, such as interaction with databases, external services and other systems. 

To better understand how this layer works, we will explore how the mongo `restaurant.postgre.repository.ts` repository would be implemented using the prism ORM.

```ts title="Restaurant.postgre.repository.ts"
export class RestaurantPostgresRepository implements RestaurantRepositoryPort {
    private prisma: PrismaClient;
    constructor() {
        this.prisma = prisma;
    }

    async findOneById(id: ID): Promise<Restaurant | null> {
        const restaurantFound: RestaurantDto =
            await this.prisma.restaurant.findUnique({
                where: { id: id.value },
            });

        if (!restaurantFound) {
            return null
        }

        const restaurant: Restaurant = RestaurantMapper.toDomain(restaurantFound);

        return restaurant;
	}

}
```

1. **Database Access:** The infrastructure layer uses Prisma to access the database and retrieve the restaurant information. A query is executed to find a unique restaurant based on the ID provided.
    
2. **Result Handling:** If no restaurant with the provided `ID` is found, the function returns null. Otherwise, it maps the found restaurant data to a Restaurant domain object.
    
3. **Result Delivery:** Finally, it returns the found restaurant or null as the result of the function.

#### Presentation
In the hexagonal architecture, the presentation layer plays a key role in providing an interface for users to interact with the system through different channels, such as REST APIs, web pages or user interfaces.

En el siguiente ejemplo, se muestra cómo se podría implementar un controlador en la capa de presentación utilizando TypeScript y NestJS para gestionar las operaciones relacionadas con los restaurantes:
```ts title="Restaurant.controller.ts"
@ApiTags('restaurant')
@Controller('restaurant')
export class RestaurantController {
    constructor(
        @Inject(RestaurantPostgresRepository)
        private readonly restaurantPostgresRepository: RestaurantPostgresRepository,
    ) {}

    @Get(':id')
    @ApiResponse({
        status: 200,
        description: 'The restaurant has been successfully retrieved.',
        type: RestaurantDto,
    })
    async getRestaurantById(
        @Param('id') id: string,
    ): Promise<RestaurantDto> {
        const retrieveRestaurant = new RetrieveRestaurantUseCase(
            this.restaurantPostgresRepository,
        );

        const restaurantDTO = await retrieveRestaurant.run(id);

        return restaurantDTO;
    }
}
```

1. **Endpoints definition:** The controller defines different endpoints to handle requests related to restaurants. In this case, a `GET` endpoint is defined which allows retrieving a restaurant by its 

2. **Request Handling:** When a request is received at the `GET /restaurant/:id` endpoint, the controller extracts the restaurant ID from the request parameters and uses it to create an `ID` object.
  
3. **Use Case Execution:** Next, the `RetrieveRestaurantUseCase` use case is instantiated, passing it the restaurant repository. This use case is in charge of retrieving the corresponding restaurant using the ID. 

4. **Result Delivery:** Finally, the controller returns the retrieved restaurant in the form of a `RestaurantDto` object in response to the request.
