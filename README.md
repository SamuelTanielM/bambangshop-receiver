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
-   [X] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [X] Commit: `Create Notification model struct.`
    -   [X] Commit: `Create SubscriberRequest model struct.`
    -   [X] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [X] Commit: `Implement add function in Notification repository.`
    -   [X] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [X] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 3: Implement services and controllers**
    -   [X] Commit: `Create Notification service struct skeleton.`
    -   [X] Commit: `Implement subscribe function in Notification service.`
    -   [X] Commit: `Implement subscribe function in Notification controller.`
    -   [X] Commit: `Implement unsubscribe function in Notification service.`
    -   [X] Commit: `Implement unsubscribe function in Notification controller.`
    -   [X] Commit: `Implement receive_notification function in Notification service.`
    -   [X] Commit: `Implement receive function in Notification controller.`
    -   [X] Commit: `Implement list_messages function in Notification service.`
    -   [X] Commit: `Implement list function in Notification controller.`
    -   [X] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1

- >In this tutorial, we used RwLock<> to synchronise the use of Vec of Notifications. Explain why it is necessary for this case, and explain why we do not use Mutex<> instead?

  Penggunaan RwLock<> diperlukan dalam kasus ini karena kita ingin memungkinkan multiple read operations (baca) secara bersamaan tanpa blocking, namun hanya satu write operation yang dapat dilakukan pada suatu waktu. Dalam konteks notifikasi, kita ingin memungkinkan banyak subscriber untuk membaca notifikasi secara bersamaan tanpa ada konflik, namun ketika ada notifikasi baru yang ditambahkan ke dalam Vec, hanya satu thread yang dapat menambahkan notifikasi tersebut. RwLock<> memungkinkan ini dengan cara memberikan akses eksklusif untuk operasi penulisan, sementara masih memungkinkan akses bersama untuk operasi pembacaan. Menggunakan Mutex<> dalam kasus ini akan mengakibatkan blocking, di mana setiap operasi (baik baca maupun tulis) akan memerlukan penguncian yang eksklusif, sehingga hanya satu operasi yang dapat dilakukan pada suatu waktu, yang dapat menyebabkan penurunan kinerja.

- >In this tutorial, we used lazy_static external library to define Vec and DashMap as a “static” variable. Compared to Java where we can mutate the content of a static variable via a static function, why did not Rust allow us to do so?

  Rust tidak mengizinkan mutasi langsung pada variabel statis karena dapat menimbulkan potensi bahaya keselamatan dan keamanan. Rust sangat memperhatikan konsep keamanan memori dan memastikan bahwa kode yang aman dari **data races** dan konflik akses memori. Jika kita diizinkan untuk secara langsung memutasi variabel statis, hal itu dapat mengakibatkan **race conditions** dan konflik akses memori, yang dapat menyebabkan kegagalan program, ketidakstabilan, atau bahkan keamanan yang rentan. Oleh karena itu, Rust mengharuskan penggunaan struktur data yang thread-safe, seperti RwLock atau Mutex, untuk memastikan bahwa akses ke variabel statis dilakukan secara aman dan terjamin. Dengan menggunakan mekanisme ini, Rust memastikan bahwa keselamatan dan keamanan program tetap terjaga.

#### Reflection Subscriber-2

- >Have you explored things outside of the steps in the tutorial, for example: src/lib.rs? If not, explain why you did not do so. If yes, explain things that you have learned from those other parts of code.

  Dari penelusuran kode yang diberikan, kita melihat penggunaan lazy_static untuk inisialisasi variabel statis, struktur AppConfig untuk konfigurasi aplikasi, penanganan kesalahan dengan ErrorResponse, penggunaan Result untuk menangani kesalahan, dan penerapan rute HTTP menggunakan framework Rocket. Ini menunjukkan praktik terstruktur dalam pengembangan aplikasi Rust, termasuk manajemen konfigurasi, penanganan kesalahan, dan pembuatan layanan web.

  Fungsi compose_error_response digunakan untuk membuat tanggapan kesalahan dengan status kode dan pesan yang diberikan.

- >Since you have completed the tutorial by now and have tried to test your notification system by spawning multiple instances of Receiver, explain how Observer pattern eases you to plug in more subscribers. How about spawning more than one instance of Main app, will it still be easy enough to add to the system?

  alam pola Observer, menambahkan pelanggan baru cukup mudah karena cukup menambahkannya ke dalam daftar pelanggan yang diamati. Namun, ketika membuat lebih dari satu instance aplikasi utama, setiap instance memiliki pola pengamatnya sendiri sehingga tidak berbagi notifikasi. Jika ingin berbagi notifikasi antar instance, diperlukan penyimpanan data bersama yang dapat diakses oleh semua instance. Hal ini membuat penambahan lebih dari satu instance menjadi lebih rumit dan membutuhkan manajemen yang hati-hati untuk memastikan koordinasi yang baik di antara mereka contohnya dengan menggunakan list.
  
- >Have you tried to make your own Tests, or enhance documentation on your Postman collection? If you have tried those features, tell us whether it is useful for your work (it can be your tutorial work or your Group Project).

    Hanya dikit, salah satu fitur yang saya temukan berguna adalah menambahkan skrip pengujian di setiap permintaan API untuk memeriksa respon secara otomatis. Sebagai contoh, saya menambahkan skrip pengujian untuk memeriksa apakah status kode respon adalah 200 OK setelah melakukan permintaan GET untuk mendapatkan daftar produk. Ini membantu saya memastikan bahwa API berfungsi dengan baik setiap kali ada perubahan kode atau konfigurasi.