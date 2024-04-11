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
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
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

1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or **trait** in Rust) in this BambangShop case, or a single Model **struct** is enough?

Pada Observer Pattern, kebutuhan untuk mengimplementasikan interface/**trait**, bergantung kepada implementasi untuk subscriber. Pada kasus BambangShop, penggunaan interface/**trait** tidak diperlukan karena semua Subscriber diwakili oleh satu class sehingga semua Subscribernya dianggap memiliki behavior yang sama. Implementasi **trait** akan menjadi penting, jika terdapat Subscriber(Observer) baru yang memiliki behavior berbeda.

2. **id** in **Product** and **url** in **Subscriber** is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using **DashMap** (map/dictionary) like we currently use is necessary for this case?

Pada kasus disini dimana id serta url dibuat unik, menggunakan **DashMap** akan lebih baik karena dengan menggunakan **url** atau **id** sebagai key lebih mudah dibandingkan menggunakan Vec. Jika kita lebih memilih menggunakan Vec, misal pada kasus url yang serupa dengan id, kita harus menggunakan 2 Vec terpisah untuk menyimpan url dan subsciber. tentu itu akan lebih merepotkan dalam proses pengelolaan data serta memperlambat proses pula. Oleh karena itu, penggunaan **DashMap** akan lebih baik jika **id** pada **Product** dan **url** di **Subscriber** unik pada kasus ini.

3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?

Menurut saya, penggunaan DashMap tetap diperlukan dibandingkan HashMap untuk memastikan ThreadSafe dalam lingkungan multithreading. Selain itu Singleton juga diperlukan untuk memastikan instance SUBSCRIBER dari DashMap hanya terdapat satu untuk diakses banyak Thread. Jadi dengan kombinasi tersebut diharapkan daftar subscriber hanya terdapat satu di DashMap dan tidak tersebar-sebar.

#### Reflection Publisher-2

1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?

Berdasarkan konsep yang telah dipelajari sebelumnya yaitu *Separation Of Concerns* dalam SOLID Principle, dijelaskan bahwa memisahkan tiap bagian suatu bagian berdasarkan fungsinya masing-masing sangatlah penting dalam mencapai kode yang mudah fleksibel untuk dikelola secara spesifik. Dengan menggunakan konsep tersebut pada pengaplikasian kasus ini, *Repository* akan berguna untuk penyimpanan dan akses data, sedangkan *service* untuk logika bisnis. Oleh karena itu, pemisahan berdasarkan SRP untuk "Service" serta "Repository" dari Model cukup penting disini demi mencapai kode yang maintanable dan mudah dikelola.

2. What happens if we only use the Model? Explain your imagination on how the interactions between each model (**Program, Subscriber, Notification**) affect the code complexity for each model?

Jika hanya menggunakan model, kode akan tetap bekerja namun kompleksitas dari kode tersebut akan tidak fleksibel disebabkan oleh terkaitnya suatu bagian dengan bagian yang lain. Hal ini dapat menyebabkan kode kita tidak maintanable dan kompleksitas kodenya menjadi cukup tinggi dalam sebuah model saja. Jadi, jika tidak terdapat pemisahan antara "Service" dan "Repository" dari model akan menyebabkan kode tidak maintanable serta kompleksitasnya cukup tinggi.

3. Have you explored more about **Postman**? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.

Sudah, sebelumnya pada mata kuliah Pemrograman Berbasis Platform *Postman* sudah pernah diperkenalkan untuk mempermudah mengirimkan http request dengan suatu data pada body ataupun parameter kepada suatu url/endpoint. *Postman* sangat membantu saya dalam memeriksa apakah suatu endpoint dapat menerima data yang saya berikan dengan baik dan apakah return data juga dikembalikan dengan baik. 

#### Reflection Publisher-3

1. Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?

Pada kasus tutorial ini, Observer Pattern yang digunakan adalah *Push Model* untuk sistem notifikasi setiap proses add, delete dan publish product. Ketika terdapat proses-proses tersebut, NotificationService akan melakukan pula proses pengiriman notifikasi kepada semua subscriber yang telah berlangganan kepada *product_type* yang bersangkutan.  

2. What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)

Jika kita menggunakan metode lain yaitu pull model untuk kasus Subscriber pada tutorial ini, maka Subscriber haruslah aktif untuk meminta data kepada Publisher. Keuntungan dari hal tersebut adalah Subscriber dapat mengambil data yang relevan dengan mereka kapanpun, namun hal tersebut juga bisa menjadi kekurangan karena sistem notifikasi untuk Subscriber yang sudah berlangganan harusnya untuk sistem automasi setiap terdapat add, delete, dan publish product akan berubah menjadi Subscriber yang harus mengetahui struktur dari data yang mereka ingin agar bisa mengambil data mana yang mereka mau serta waktu pengambilannya.

3. Explain what will happen to the program if we decide to not use multi-threading in the notification process.

Tanpa penggunaan multi-threading untuk proses notifikasi ini, akan sangat menghambat dikarenakan proses pengirimannya terjadi satu per satu secara berurutan. Ini akan menyebabkan lambatnya proses karena haruslah mengantri dan dapat membuat bottleneck pada sistem karena load yang cukup tinggi.
