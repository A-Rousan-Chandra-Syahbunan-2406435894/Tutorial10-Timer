# Tutorial 10 - Timer

## Experiment 1.2: Understanding how it works
![alt text](image.png)

**Penjelasan:**
Berdasarkan hasil eksekusi di atas, teks `"hey hey"` dicetak lebih dulu dibandingkan `"howdy!"`, padahal di dalam kode, fungsi `spawner.spawn(...)` yang berisi `"howdy!"` dipanggil lebih awal daripada perintah `println!("... hey hey")`. 

Hal ini terjadi karena cara kerja *Asynchronous Programming* di Rust (khususnya sifat *Futures* yang *lazy*):

1. Ketika `spawner.spawn(...)` dipanggil, *task* (tugas) berupa *async block* tersebut **tidak langsung dieksekusi**. *Spawner* hanya membungkus tugas tersebut dan memasukkannya ke dalam antrean (sebuah *channel/queue*).
2. Karena eksekusi program utama bersifat *synchronous* dan tidak terblokir (non-blocking) oleh proses `spawn`, program langsung lanjut mengeksekusi baris berikutnya di fungsi `main`, yaitu mencetak `"hey hey"`.
3. *Task* yang kita *spawn* tadi baru akan benar-benar dijalankan ketika kita memanggil fungsi `executor.run()`. Di sinilah executor mulai menarik *task* dari antrean dan mengeksekusinya (*polling*), sehingga `"howdy!"` baru dicetak, lalu menunggu 2 detik, dan diakhiri dengan mencetak `"done!"`.
