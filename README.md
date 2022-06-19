# Intro-to-AI - Mediapipe Assignment

## Introduction

Pada assignment ini, saya ingin coba untuk implementasi apakah seseorang bisa mengikuti 3 pose pada perkenalan diri (Jikoshoukai) Ella JKT48 atau tidak. Berikut adalah perkenalan diri Ella JKT48:

[![Jikoshoukai Ella JKT48](https://img.youtube.com/vi/pTPnmyzkBUw/0.jpg)](https://www.youtube.com/watch?v=pTPnmyzkBUw)

Dari video di atas, kita bisa lihat ada 3 pose yang dilakukan oleh Ella JKT48 ketika dia memperkenalkan dirinya:
- Ohayo, dengan tangan diangkat serong ke kanan atas dengan telapak tangan dibuka,
- Konnichiwa, dengan tangan kanan dikepalkan dan bertumpu pada pinggang,
- Oyasumi, dengan telapak tangan dibuka lebar dan ditempelkan pada pipi kanan, lengan ditempelkan pada tubuh.


Berdasarkan 3 pose itu, ada beberapa titik yang kita harus hitung sudutnya untuk mendapatkan akurasi perkenalan diri Ella JKT48 (lihat gambar di bawah):
- Ohayo, membutuhkan sudut ≈180° di titik 14 dan sudut 150°<x<170° di titik 12,
- Konnichiwa, membutuhkan sudut 100°<x<130° di titik 14 dan sudut 120°<x<130° di titik 12,
- Oyasumi, membutuhkan sudut <40° di titik 14 dan sudut 100°<x<130° di titik 12.

Namun untuk mentrigger ketiga pose tersebut, tangan kiri harus ikut berpose memegang microphone dengan sudut 10°<x<50° di titik 13 dan 90°<x<130° di titik 11.

<img src="https://i.imgur.com/3j8BPdc.png" style="height:300px" >

## Video capture, rendering calculation, and render stage state

Kita create dulu mediapipe instance yang akan kita sebut sebagai pose. Nah di dalam instance ini akan ada proses yang berulang (loop) selagi webcam kita masih terbuka.

Lalu di dalam loop ini, proses berikut akan terus berulang hingga kita pencet tombol Q untuk menutup jendela webcamnya:

1. Kita ambil dulu frame dari webcamnya.
2. Kita konversi gambar frame tersebut dari BGR ke RGB (konvensi dari openCV, defaultnya adalah BGR) dan dimasukkan ke variabel image
3. Kita proses dan deteksi "skeleton" dari gambar tadi dan dimasukkan ke variabel results
4. Image kita kembalikan dari RGB ke BGR
5. Kita coba ekstrak landmark yang ada di results, dan jika tidak berhasil, maka akan lompat ke langkah berikutnya:

        a. Kita ambil semua landmark di results dan masukkan ke variable landmarks
        b. Kita ambil semua koordinat landmark yang kita butuhin (bahu kiri dan kanan, siku kiri dan kanan, pergelangan tangan kiri dan kanan)
        c. Kita hitung sudut pada titik 11, 12, 13, dan 14
        d. Kita render sudut di masing-masing titik
        e. Kita ganti state dari stage sesuai sudut-sudut yang terkalkulasi sebelumnya sesuai dengan logika yang ada di bawah
6. Kita render sebuah kotak, tulisan stage, dan stage statenya
7. Kita render seluruh landmark yang sudah kita "kerjakan" sebelumnya
8. Tampilkan dalam window
9. Ulangi proses hingga user/kita menekan tombol q

<img src="https://github.com/emilirgi18/Intro-to-AI---Mediapipe/blob/main/Untitled%20Diagram.drawio.png?raw=true">

## Example

[![Contoh Hasil](https://img.youtube.com/vi/507ZVx5Vl1g/0.jpg)](https://www.youtube.com/watch?v=507ZVx5Vl1g)
