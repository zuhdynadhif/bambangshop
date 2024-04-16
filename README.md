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

    > Dalam Observer design pattern yang sudah dipelajari, penggunaan interface memberikan beberapa keuntungan yang terkait dengan aspek non-functional seperti fleksibilitas dan kemudahan dalam pengembangan. 
    
    > Dalam kasus BambangShop, penggunaan interface tidak diperlukan karena tidak ada kebutuhan untuk mengimplementasikan beberapa jenis Subscriber yang berbeda. Dalam kasus ini, cukup menggunakan struct Subscriber yang sudah ada.
    
    > Meskipun begitu, penggunaan interface tetap bisa dilakukan untuk mempermudah pengembangan di masa depan jika diperlukan.

2. id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?

    > Penggunaan `Dashmap` sebagai map/dictionary lebih cocok digunakan dalam kasus ini karena memungkinkan untuk memastikan keunikan dan juga pengaksesan data dengan lebih cepat. Sebaliknya, penggunaan `Vec` sebagai list tidak memungkinkan untuk memastikan keunikan dan juga performa pengaksesan datanya lebih lambat.

3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?

    > Penggunaan `Dashmap` sebagai thread-safe HashMap merupakan pilihan yang tepat karena memungkinkan untuk mengakses data secara aman dari thread tanpa harus melakukan setup secara manual. Hal ini jauh lebih menguntungkan dibandingkan dengan penggunaaan Singleton pattern yang memerlukan implementasi manual untuk memastikan thread-safety.

#### Reflection Publisher-2
1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?

    > Pemisahan antara business logic dan juga data storage ditujukan untuk menerapkan separation of concerns, dimana setiap komponen memiliki tanggung jawab yang jelas dan terpisah. Dengan pemisahan ini, kode akan meningkatkan maintainability dari aplikasi.

2. What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?

    > Jika kita hanya menggunakan model saja tanpa ada pemisahan business logic dan juga data storage akan menyebabkan tingginya kompleksitas program dan juga membuat adanya coupling antar model. Kompleksitas akan meningkat karena dalam satu blok kode, harus mengandung business logic dan instance repository. Hal ini akan menyulitkan pengembangan dan proses maintenance dari program karena kode yang tidak terstruktur dan berserakan.

3. Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.

    > Postman sangat membantu saya dalam mengelola API, terutama dalam melakukan validasi apakah suatu API sudah sesuai atau belum. Postman juga mampu menampilkan data dalam interface yang intuitif dan mudah dipahami. Fitur yang paling saya sukai adalah kemampuan untuk membuat collection dan environment yang memungkinkan saya untuk mengelola dan mengatur API yang akan diuji sesuai dengan kebutuhan tertentu.

#### Reflection Publisher-3

1. Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?

    > Kasus ini menggunakan push model. Hal ini terlihat pada penggunaan fungsi `notify`, dimana publisher memberikan notifikasi kepada subscriber yang ada pada `SubscriberRepository`. Untuk setiap subscriber yang ada, publisher akan mengirimkan notifikasi dengan melakukan pemanggilan dari method update yang ada pada Subscriber dengan payload yang sesuai. Ini merupakan mekanisme Push Model pada Observer Pattern.

2. What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)

    > Keuntungan dari implementasi ini adalah pengurangan overhead dan data retrieval control yang lebih baik. Subscriber hanya akan melakukan request update ketika memang dibutuhkan. Namun, kekurangannya adalah peningkatan kompleksitas pada implementasi dikarenakan adanya kebuthkan untuk mengimplementasikan polling mechanism pada subscriber.

3. Explain what will happen to the program if we decide to not use multi-threading in the notification process.

    > Jika tidak digunakan, amka akan muncul blocking behavior karena notifikasi yang dikirimkan ke subscriber adalah sebuah sequencial program. Sehingga, tidak tidak mengaplikasikan multithreading, maka setiap subscriber harus mengantri. Karenanya, program tidak akan responsive dan kenyamanan pengguna akan menurun.