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
-   [v] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Subscriber model struct.`
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [x] Commit: `Implement add function in Subscriber repository.`
    -   [x] Commit: `Implement list_all function in Subscriber repository.`
    -   [x] Commit: `Implement delete function in Subscriber repository.`
    -   [x] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [x] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [x] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [x] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [x] Commit: `Implement publish function in Program service and Program controller.`
    -   [x] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [x] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?

   Satu Model struct sudah cukup karena semua subscriber di BambangShop berperilaku sama. Namun jika ingin menambahkan perilaku baru pada subscriber baru BambangShop, perlu interface atau trait agar lebh fleksibel dan mudah diikembangkan.
   
2. id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?

   Karena id dan url unique, DashMap lebih efisien untuk digunakan karena data disimpan dengan key-value dan kompleksitas pencariannya O(1). Jika pakai list, pencarian harus dilakukan iterasi satu-satu O(n), sehingga list menjadi kurang efisien terutuama jika data banyak.

3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?
   
   Singleton hanya memastikan bahwa SUBSCRIBERS memiliki satu instance global, tetapi tidak menjamin thread safety saat diakses oleh banyak thread. Sementara itu, DashMap ada HashMap yang sudah thread-safe sehingga dapat diakses dan dimodifikasi secara aman secara paralel. Karena itu, meskipun menggunakan Singleton, DashMap tetap diperlukan untuk mencegah race condition.

#### Reflection Publisher-2
1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?

   Karena jika tidak dipisah, hal itu melanggar SOLID principle, yaitu SRP, di mana sebuah komponen hanya mempunyai satu tanggung jawab. Memisahkan keduanya juga mempermudah testing dan maintainability.

2. What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?

    Jika hanya menggunakan model, tiap model harus menangani banyak tanggung jawab sekaligus, seperti logika bisnis, pengelolaan data, dan interaksi dengan model lain. Hal ini membuat model saling bergantung satu sama lain sehingga kode menjadi sulit untuk testing dan sulit dipelihara.

3. Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.

    Postman sangat membantu saya untuk mengecek apakah API yang dibuat sudah sesuai dengan yang diharapkan. Dengan Postman, saya dapat mengirim berbagai jenis request untuk melihat apakah endpoint API bekerja dengan benar dan memeriksa response yang diberikan server. Lalu fitur seperti environment variables membantu mengatur URL dengan lebih mudah.

#### Reflection Publisher-3
1. Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?

   Pada tutorial ini, variasi yang digunakan adalah push model karena cara kerja fungsi notify() di NotificationService mengirim data notifikasi ke tiap subscriber melalui HTTP POST secara langsung.

2. What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)

   Keuntungan dari memakai pull model adalah subscriber bisa mengatur kapan ingin mengambil notifikasi sehingga lebih fleksibel dan hanya mengambil saat benar-benar dibutuhkan. Kekurangannya adalah subscriber harus mengecek sendiri untuk mendapatkan notifikasi terbaru sehingga dapat menyebabkan keterlambatan menerima update.

3. Explain what will happen to the program if we decide to not use multi-threading in the notification process.

    Jika tidak menggunakan multi-threading, pengiriman notifikasi akan dilakukan satu-satu sehingga jika jumlah subscriber banyak, subscriber harus menunggu subscriber lain hingga prosesnya selesai, sehingga akan ada subscriber yang menerima notifikasi dengan lebih lambat.
   



