**a.** `AMQP` (Advanced Message Queuing Protocol) adalah sebuah protokol *open standard* untuk *message-oriented middleware*, yang memungkinkan aplikasi saling bertukar *message* secara asynchronous melalui *message broker* seperti RabbitMQ.

**b.** Pada URL `amqp://guest:guest@localhost:5672`, bagian pertama `guest` adalah *username*, bagian kedua `guest` adalah *password*, sedangkan `localhost:5672` menunjukkan *host* (komputer lokal) dan *port* 5672, yaitu *port* default yang digunakan RabbitMQ untuk menerima koneksi AMQP. Jadi, URL ini digunakan oleh aplikasi untuk terhubung ke RabbitMQ yang berjalan secara lokal menggunakan kredensial default.


## Simulation slow subscriber
![/run-simulation](./images/simulation-one.png)
Dalam kasus ini, jumlah pesan yang masuk ke antrian mencapai 20. Hal ini terjadi karena adanya penundaan buatan di dalam kode subscriber dengan perintah `thread::sleep(ten_millis);`, yang menyebabkan subscriber hanya memproses satu pesan setiap 10 milidetik. Sementara itu, producer dapat mengirimkan banyak pesan dalam waktu yang jauh lebih singkat. Akibatnya, pesan-pesan tersebut menumpuk sementara di antrian.


## Running at least three subscribers
![/run-simulation](./images/simulation-three.png)
Setelah menjalankan aplikasi subscriber sebanyak tiga kali, grafik pada RabbitMQ menunjukkan antrean pesan (`queued messages`) berkurang lebih cepat, dan pesan yang dikirim oleh publisher segera diproses tanpa penumpukan signifikan. Hal ini menunjukkan bahwa dengan menambah jumlah subscriber, kecepatan konsumsi pesan meningkat dan sistem menjadi lebih responsif terhadap lonjakan pesan yang dikirim secara bersamaan. Dengan demikian, menjalankan beberapa instance subscriber secara bersamaan merupakan solusi efektif untuk mengurangi `bottleneck`.
