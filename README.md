# Jarkom-Modul-4-2025-K58

## CIDR Calculation

Link CIDR calculation: https://cryptpad.disroot.org/sheet/#/2/sheet/view/F8AG5XtoutJzq5eTlJp4b-WXt5N7Et7QBZmAiBHBpSk/

Things to do/watchout for:
1. Firstly, map the demanded host, net mask, net addr, usable ip range, and broadcast addr of every subnet to a spreadsheet.
2. Supernet it from either top/bottom, NOT MIDDLE.
3. Draw the supernet as a tree or whatever you fancy to help visualize.
4. Use subnetmask table with its binary representation included too.
5. Router subnets dont have to be combined with host subnet one by one, eg. here A1 -> A2 -> A3 -> Internet:

```
Host subnet: A1
Router subnet 1: A2
Router subnet 2: A3

Subnet goes like: B1 = A1, A2, A3

NOT: B1 = A1, A2; B2 = B1, A3
```

4. Careful with routing's prefix assignment and its matching subnet, eg. if B1 is 192.240.0.0/22, then B2 must be started from 192.240.4.0/22 or higher. This extends into supernet's calculation process where it must pay attention to prev supernet for same reason.

6. In a supernet, subnets goes contiguously (means addr of one subnet continues to other subnet like a chained link).

```
A1: 192.240.0.128/25
A2: 192.240.1.0/30
A3: 192.240.1.4/29
A4: 192.240.4.0/22

Not a good idea since there is a gap between A3-A4
```

## Langkah-Langkah Pengerjaan VLSM

Berikut adalah proses 4 langkah yang kita terapkan:

## Link hasil

https://docs.google.com/spreadsheets/d/10H_4rIy1i-TZMVnZefXNsIQKMRqATaR5-aOwLks5vXs/edit?usp=sharing

### 1.  Identifikasi Kebutuhan Host

Langkah pertama adalah melihat topologi dan mencatat *semua* kebutuhan jaringan.
* **Jaringan LAN:** Kita mencatat 27 jaringan LAN, contohnya:
    * **Mirluin:** butuh 628 host
    * **Silmarils:** butuh 381 host
    * **Erebor:** butuh 2 host
* **Jaringan WAN:** Kita mencatat 13 link *point-to-point* (koneksi antar router). Jaringan ini **selalu membutuhkan 2 host**.
    * Contoh: `Amonsul <-> Minastir`, `Eregion <-> Numenor`, dll.

### 2. Hitung Ukuran Blok IP 

Kita menghitung total IP yang dibutuhkan untuk setiap jaringan, lalu membulatkannya ke bilangan pangkat 2 terdekat.

> **Contoh dari Data Kita:**
>
> * **Kebutuhan:** **Mirluin** (628 host)
>     * Perhitungan: 628 host + 2 IP (Network & Broadcast) = 630
>     * Ukuran Blok: Dibulatkan ke atas menjadi **1024** (Prefix /22)
>
> * **Kebutuhan:** **Morgul** (102 host)
>     * Perhitungan: 102 + 2 = 104
>     * Ukuran Blok: Dibulatkan ke atas menjadi **128** (Prefix /25)
>
> * **Kebutuhan:** **Link WAN** (2 host)
>     * Perhitungan: 2 + 2 = 4
>     * Ukuran Blok: **4** (Prefix /30)

### 3. Urutkan dari Terbesar ke Terkecil

Ini adalah **aturan paling penting** dalam VLSM. Kita mengurutkan semua (27 LAN + 13 WAN) jaringan tersebut berdasarkan Ukuran Blok-nya, dari terbesar ke terkecil.

* **Paling Atas:** **Mirdain** (Blok 1024 / /22)
* **Di Tengah:** **Morgul** (Blok 128 / /25), **Palantir** (Blok 64 / /26), dst.
* **Paling Bawah:** **Erebor**, **Utumno**, dan **13 Link WAN** (Semuanya Blok 4 / /30)

### 4. Lakukan Alokasi IP

Terakhir, kita bagikan IP-nya secara berurutan, dimulai dari network induk **192.240.0.0**.

1.  **Untuk Jaringan Pertama (Mirluin /22):**
    * Network ID: `192.240.0.0`
    * Broadcast: `192.240.3.255`
    * *Alokasi ini menghabiskan IP dari 192.240.0.0 s/d 192.240.3.255.*

2.  **Untuk Jaringan Kedua (Silmarils /23):**
    * Kita mulai dari IP selanjutnya yang tersedia: `192.240.4.0`
    * Network ID: `192.240.4.0`
    * Broadcast: `192.240.5.255`

3.  **...Proses ini diulang terus...**
    * Kita terus mengalokasikan IP untuk **semua 27 LAN**, sampai jaringan LAN terkecil.
    * Alokasi LAN terakhir (Erebor /30) mendapatkan Network ID: `192.240.18.180` dan berakhir di `192.240.18.183`.

4.  **Untuk Jaringan WAN (Semua /30):**
    * Kita melanjutkan dari IP terakhir yang tersedia: `192.240.18.184`.
    * Link WAN pertama (`Amonsul <-> Minastir`) mendapat Network ID: `192.240.18.184`.
    * Link WAN kedua (`Minastir <-> Amroth`) mendapat Network ID: `192.240.18.188`.
    * ...dan seterusnya sampai semua 13 link WAN teralokasi.

Alokasi terakhir kita adalah untuk link `Fornost <-> Valmar` di `192.240.18.232`. Alamat IP berikutnya yang tersedia untuk ekspansi adalah `192.240.18.236`.
