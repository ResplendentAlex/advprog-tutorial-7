# BambangShop Publisher App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

---

## About this Project
In this repository, we have provided you a REST (REpresentational State Transfer) API project using Rocket web framework.

This project consists of four modules:
1.  `controller`: this module contains handler functions used to receive request and send responses.
    In Model-View-Controller (MVC) pattern, this is the Controller part.
2.  `model`: this module contains structs that serve as data containers.
    In MVC pattern, this is the Model part.
3.  `service`: this module contains structs with business logic methods.
    In MVC pattern, this is also the Model part.
4.  `repository`: this module contains structs that serve as databases and methods to access the databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a basic functionality that makes BambangShop work: ability to create, read, and delete `Product`s.
This repository already contains a functioning `Product` model, repository, service, and controllers that you can try right away.

As this is an Observer Design Pattern tutorial repository, you need to implement another feature: `Notification`.
This feature will notify creation, promotion, and deletion of a product, to external subscribers that are interested of a certain product type.
The subscribers are another Rocket instances, so the notification will be sent using HTTP POST request to each subscriber's `receive notification` address.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    APP_INSTANCE_ROOT_URL="http://localhost:8000"
    ```
    Here are the details of each environment variable:
    | variable              | type   | description                                                |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [ ] Commit: `Create Subscriber model struct.`
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Subscriber repository.`
    -   [ ] Commit: `Implement list_all function in Subscriber repository.`
    -   [ ] Commit: `Implement delete function in Subscriber repository.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [ ] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [ ] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [ ] Commit: `Implement publish function in Program service and Program controller.`
    -   [ ] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?

To my knowledge of Observer design patterns, and the SOLID principles, I think that the use of interface is still important and essentially needed in this context. This is because it will help us create a codebase environment that is much more extensible and scalable, ensuring that we follow the "**Open/Closed Principle**" of SOLID. Aside from that, we could also implement the "**Dependency Inversion**" into our codebase as the Publisher will not be dependent on the concrete implementation of the Subscriber, but rather the Subscriber interface. This allows the codebase to be more modular and easier to maintain in the long run. 

2. id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?

If both the id and the url of the Subscriber is unique, then the useage of DashMap would allow for a quicker lookup operation, reducing the time it takes to finish the task. This is because the DashMap loopup implementation allows the operation to run in O(1) time complexity, while Vec runs in O(n) time complexity. This shows just how much the useage of DashMap is important, as we know that when the value of n increases to a huge number, the time taken for each loopup operation done by Vec would also increase linear to the amount of increase in the value of n. With DashMap, however, there will be no change in the time taken for each operation, thus ensuring the implementation to be the most efficient possible.

3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?

I think that it is best if we use both DashMap and Singleton pattern in the same project. While the Singleton pattern may be able to use the lazy_static to provide a safe access to HashMap datas, it does not prevent other issues that may arrise to concurrent access to the HashMap. The DashMap, instead, helps us in providing a safe access that runs concurrently, thus allowing concurrent access.

#### Reflection Publisher-2
1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?

It all comes down to the compliance of one of the SOLID principles, which is the "**Single Responsibility Principle (SRP)**", as well as the "**Separation of Concerns**" principle. We know that the Model in MVC is meants as a data representation. By separating it to the Service and Repoistory, we can divide it into two different roles, where the Repository handles data access and modification layer, while the Service handles the business logic. By doing this, we could effectively separate concerns and responsibilities of each component, allowing the codebase to be more modular. 

2. What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?

If we only use the Model, then the codebase would be less modular than and harder to maintain. This is because the Model would handle two different responsibilities, that is the data storage and business logic, which would violate the SRP. This may, in turn, leads to an increase in code complexity and even tighter coupling in the codebase.

3. Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.

I've used Postman in a previous project before. It is especially helpful for me to check whether my implemented APIs have already hit the correct endpoints, and whether my HTTP requests have been successfully accomplished. This allows me to be able to quickly navigate between APIs and do very quick testing on my projects, increasing my overall workflow, as well as increasing my reducing the amount of workload I need to do. Since I have already been using it previously, it would be a shame not to use in future projects, since it has provided me with ease in sharing APi documentation with other developers.

#### Reflection Publisher-3
