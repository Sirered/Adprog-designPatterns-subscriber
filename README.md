# BambangShop Receiver App
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
4.  `repository`: this module contains structs that serve as databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a Rocket web framework skeleton that you can work with.

As this is an Observer Design Pattern tutorial repository, you need to implement a feature: `Notification`.
This feature will receive notifications of creation, promotion, and deletion of a product, when this receiver instance is subscribed to a certain product type.
The notification will be sent using HTTP POST request, so you need to make the receiver endpoint in this project.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Receiver" folder.

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    ROCKET_PORT=8001
    APP_INSTANCE_ROOT_URL=http://localhost:${ROCKET_PORT}
    APP_PUBLISHER_ROOT_URL=http://localhost:8000
    APP_INSTANCE_NAME=Safira Sudrajat
    ```
    Here are the details of each environment variable:
    | variable                | type   | description                                                     |
    |-------------------------|--------|-----------------------------------------------------------------|
    | ROCKET_PORT             | string | Port number that will be listened by this receiver instance.    |
    | APP_INSTANCE_ROOT_URL   | string | URL address where this receiver instance can be accessed.       |
    | APP_PUUBLISHER_ROOT_URL | string | URL address where the publisher instance can be accessed.       |
    | APP_INSTANCE_NAME       | string | Name of this receiver instance, will be shown on notifications. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)
3.  To simulate multiple instances of BambangShop Receiver (as the tutorial mandates you to do so),
    you can open new terminal, then edit `ROCKET_PORT` in `.env` file, then execute another `cargo run`.

    For example, if you want to run 3 (three) instances of BambangShop Receiver at port `8001`, `8002`, and `8003`, you can do these steps:
    -   Edit `ROCKET_PORT` in `.env` to `8001`, then execute `cargo run`.
    -   Open new terminal, edit `ROCKET_PORT` in `.env` to `8002`, then execute `cargo run`.
    -   Open another new terminal, edit `ROCKET_PORT` in `.env` to `8003`, then execute `cargo run`.

## Mandatory Checklists (Subscriber)
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create SubscriberRequest model struct.`
    -   [ ] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Notification repository.`
    -   [ ] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [ ] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 3: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Commit: `Implement receive_notification function in Notification service.`
    -   [ ] Commit: `Implement receive function in Notification controller.`
    -   [ ] Commit: `Implement list_messages function in Notification service.`
    -   [ ] Commit: `Implement list function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1

1. We use RwLock, instead of Mutex, because we expect the frequency of reads to be a lot higher than writes, since we would expect many users trying to read the notifications at the same time, so many concurrent reads will occur, while writes should only occur whenever a change is recieved from the publisher. The reason that RwLock is better than Mutex in such scenarios, because it will allow multiple concurrent reads to happen at a time, while still providing thread safety, by making sure that when a write happens, no reads or writes can happen at the same time, whereas Mutex will lock the Vector and prevent access, if any other thread is currently writing OR reading to the Vector, thus disallowing concurrent reads (since with Mutex, even reads will make it so other threads can't access the Vector until the read is done).


2. Rust's philosophy is that they force programmers to correctly handle problems such as memory safety, handling race conditions and defining behaviour in multi-threaded environments in their code, otherwise the code won't compile. This is as opposed to Java, where such issues don't immediately stop compilation, thus possible logical errors that are more difficult to identify than compile errors may occur. In support of Rust's philosophy, all multi-threaded memory accesses is based on the idea of ownership (to a thread), borrowing and mutability, to ensure programs being made in Rust are memory safe. Lazy_static is a library that uses mutability to access data from a static object, that ensures that locks are in place on access (or just on write if you're using RwLock), which would provide memory safety and defines behaviour in multi-threaded environments. This is opposed to Java letting you use static functions to access the data, where nothing is stopping programmers from introducing buggy code that may cause data races and improper access of shared resources. If Rust were to allow us to access static variables by just making the functions static, it will go against Rust's philosophy of forcing programmers to code in a way that is memory safe and doesn't allow for ambiguous/messy behaviour. Hence why such 'fixes' are disallowed in Rust.

#### Reflection Subscriber-2

1. No, I have not, mostly because we haven't been taught what these other files do and how they work and what we can do to them without breaking the application. I mean I have read through lib/src and I understand that this is where we loaded the 'APP_' environment variables from env, which is kept in a lazy_static variable called APP_CONFIG. I also understand that this is where we do the building of the REQWEST_CLIENT, which we use to send requests. I also know that this is where we define the Result struct (which is where we wrap all our Results to check if it ran ok or if there was an error), as well as the error struct and the method that is used to compose_error.

I understand all this, but I do not see a need to mess with it, because that would be unnecessary, as the tutorial already gives a working pair of publisher and subscriber applications. Also, I am not willing to risk making changes to files I only somewhat understand (which may lead to breaking the application) to do something that isn't required. I could use this as a learning experience to understand what's going into the running of an application in Rust, but I think the tutorial and just reading lib and main is sufficient in understanding what is going on for now, and if we need to implement something more complex, then that's when I would go about messing with lib and main if necessary.

2. Observer pattern makes plugging in more subscribers easier, because all we have to do is have them make a subscription request with the topic they want to subscribe to, in which case that subscriber would be added to the dictionary of dictionary of subscribers. Then if we ever need to notify that subscriber of changes made to the topic, we already established in the code that the it will just go through all subscribers subscribed to that topic (including newly added subscribers) and call the notify function for each of those subscriber (which sends a notify request to all of these subscribers). Hence, because of the observer pattern, all we need to do is for the subscriber to subscribe to the topic and then our already established code will be able to handle it.

Similarly, running multiple instances of the main app will be similarly trivial, you would just have to set the APP_PUBLISHER_ROOT_URL of the receivers to the URL of the instance of the main app you want that receiver to communicate with. You may also configure some kind of system that will handle routing receivers to specific main app instances and then the receiver will remember the url of the app instance that it was directed to. Regardless, it shouldn't be too difficult to run multip;le instances of main app, but do keep in mind that the list of subscribers and programs of one main app may be different than another instance of the main app, because the 'database' is actually just a static dictionary related to each instance of the main app, rather than a unified database.

3. I've tried out making documentation using Postman, and it really does making documentation regarding listing down the type of requests that can be made and the expected response and behaviour is a real time-saver and quite convenient. Honestly, I didn't think about the production of such documentation before using Postman and it offering to help with making documentations on the collection page. However, after thinking about it, I now realise that this is invaluable for a user manual or for front-end developers, so I can see why this documentation is important. Postman then makes it easy to make such documentation, by allowing us to use the already established collection of requests and building off of that, thus allowing us to provide incredibly useful documentation for users/front end developers to understand what they can do and as a guide for new team members to the project. We can also add example responses in the collection which further demonstrates the (expected) behaviour of all requests.

I myself have not tried out the Testing feature of the Postman, but I've seen the process and some of the methods of Postman testing. It seems quite useful for functional testing, specifically either making sure that the response behaviour of that end point is consistent, or if there was an intentional change, the team is notified and they can change the tests, documentation and some implementation accordingly. I think the testing interface provided by Spring Boot is generally already quite sufficient, but having another suite of tests that tests the endpoints more than functionality is something that can be considered to be used in the group project, especially with more complex tests that we may have difficulty Mocking and implementing, since we are also still somewhat unfamiliar with Spring Boot testing.