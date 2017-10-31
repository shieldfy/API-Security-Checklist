[English](./README.md) | [中文版](./README-zh.md) | [Português (Brasil)](./README-pt_BR.md) | [Français](./README-fr.md) | [한국어](./README-ko.md) | [Nederlands](./README-nl.md) | [ไทย](./README-th.md) | [Русский](./README-ru.md) | [Українська](./README-uk.md) | [Español](./README-es.md) | [Italiano](./README-it.md) | [日本語](./README-ja.md) | [Deutsch](./README-de.md) | [Türkçe](./README-tr.md) | [Tiếng Việt](./README-vi.md) | [Монгол](./README-mn.md) | [हिंदी](./README-hi.md) | [العربية](./README-ar.md)

# Checklist Keamanan API
Checklist penanggulangan keamanan yang paling penting ketika merancang, menguji, dan melepaskan API ke khalayak


---

## Autentikasi
- [ ] Jangan gunakan `Basic Auth`. Gunakan autentikasi baku (Contoh: JWT, Oauth)
- [ ] Gunakan mekanisme baku untuk `autentikasi`, `pembuatan token`, dan `penyimpanan kata sandi`
- [ ] Gunakan maksimal percobaan berulang dan fitur penjara pada Login.
- [ ] Gunakan enkripsi untuk seluruh data sensitif.

### JWT (JSON Web Token)
- [ ] Gunakan kunci acak yang rumit (`JWT Secret`) untuk membuat proses pemecahan token secara paksa menjadi sangat susah.
- [ ] Jangan gunakan algoritma yang berasal dari muatan yang dikirim oleh pengguna. Paksa penggunaan algoritma di sisi peladen (`HS256` or `RS256`).
- [ ] Gunakan masa tenggat token (`TTL`, `RTTL`) yang sesingkat mungkin.
- [ ] Jangan simpan data sensitif pada muatan JWT karena muatan JWT dapat diterjemahkan [dengan mudah](https://jwt.io/#debugger-io).

### OAuth
- [ ] Selalu validasi `redirect_uri` di sisi peladen sehingga hanya URL-URL yang ada di dalam daftar putih yang boleh digunakan.
- [ ] Selalu coba untuk mempertukarkan kode bukan token (jangan ijinkan `response_type=token`).
- [ ] Gunakan parameter `state` dengan campuran nilai acak (_random hash_) untuk mencegah CSRF pada proses autentikasi.
- [ ] Tetapkan cakupan baku dan validasi parameter cakupan untuk setiap aplikasi.

## Akses
- [ ] Batasi permintaan (_throttling_) di sisi peladen untuk menghindari serangan yang dapat melumpukan sistem (Contoh: DDoS, serangan paksa).
- [ ] Gunakan HTTPS di sisi peladen untuk menghindari serangan pencegatan / MItM (Man In The Middle Attack).
- [ ] Gunakan tajuk `HSTS` pada SSL untuk mencegah serangan SSL Strip.

## Masuk
- [ ] Gunakan metode HTTP yang sesuai dengan operasi yang digunakan, `GET untuk membaca catatan`, `POST untuk membuat catatan baru`, `PUT/PATCH untuk mengganti secara keseluruhan/mengubah sebagian catatan`, `DELETE untuk menghapus catatan` dan tanggapan `405 Method Not Allowed` jika metode permintaan tidak dikenali pada sumber daya.
- [ ] Validasi `content-type` pada tajuk _Accept_ pada permintaan (Negosiasi konten) sehingga hanya mengijinkan format yang dikenali (Contoh: `application/xml`, `application/json`, dan lain sebagainya). Berikan tanggapan `406 Not Acceptable` jika nilai tajuk _Accept_ tidak dikenali.
- [ ] Validasi `content-type` dari data yang dipos oleh pengguna (Contoh: `application/x-www-form-urlencoded`, `multipart/form-data ,application/json`, dan lain sebagainya).
- [ ] Validasi masukan dari pengguna untuk menghindari kerentanan umum (Contoh: `XSS`, `SQL-Injection`, `Remote Code Execution`, dan lain sebagainya).
- [ ] Jangan gunakan data sensitif seperti `kredensial`, `kata sandi`, `token keamanan`, atau `kunci API` pada URL. Gunakan tajuk _Authorization_ baku.
- [ ] Gunakan layanan pintu gerbang API (_API Gateway_) untuk memungkinan singgahan, pembatasan laju, pendeteksian lalu lintas tinggi, dan penyebaran sumber daya API secara dinamis

## Pemrosesan
- [ ] Cek apakah seluruh titik akhir terlindungi oleh autentikasi untuk menghindari proses autentikasi yang rusak.
- [ ] Sumber daya id kepunyaan pengguna sebaiknya dihindari. Lebih baik menggunakan`/me/orders` daripada `/user/654321/orders`.
- [ ] Jangan gunakan id yang bertambah secara otomatis. Sebaiknya gunakan `UUID`.
- [ ] Jika hendak menguraikan berkas XML, pastikan penguraian entitas tidak diaktikan untuk menghindari serangan `XXE` (XML External Entity).
- [ ] Jika hendak menguraikan berkas XML, pastikan perluasan entitas tidak diaktifkan untuk menghindari `Billion Laughs/XML bomb` melalui serangan perluasan entitas eksponensial.
- [ ] Gunakan CDN untuk unggah berkas.
- [ ] Jika berhubungan dengan jumlah data yang sangat besar, gunakan Pekerja dan Antrian untuk memproses sebanyak mungkin di balik layar dan kembalikan tanggapan cepat untuk menghindari pemblokiran HTTP.
- [ ] Jangan lupa untuk mematikan mode DEBUG.

## Keluaran
- [ ] Kirim tajuk `X-Content-Type-Options: nosniff`.
- [ ] Kirim tajuk `X-Frame-Options: deny`.
- [ ] Kirim tajuk `Content-Security-Policy: default-src 'none'`.
- [ ] Hapus tajuk sidik jari - `X-Powered-By`, `Server`, `X-AspNet-Version` dan lain sebagainya.
- [ ] Paksa `content-type` pada tanggapan. Jika mengambalikan `application/json` maka tajuk `content-type` adalah `application/json`.
- [ ] Jangan kembalikan data sensitif seperti `kredensial`, `kata sandi`, dan `token keamanan`.
- [ ] Kembalikan kode status yang layak sesuai dengan operasi yang diselesaikan (Contoh: `200 OK`, `400 Bad Request`, `401 Unauthorized`, `405 Method Not Allowed`, dan lain sebagainya).

## CI & CD
- [ ] Audit rancangan dan pelaksanaan dengan pengujian unit/integrasi.
- [ ] Gunakan proses ulasan kode dan kesampingkan persetujuan sendiri.
- [ ] Pastikan seluruh komponen layanan dipindai secara statis menggunakan anti virus sebelum didorong ke lingkungan produksi, termasuk pustaka-pustaka milik vendor dan ketergantungan lainnya.
- [ ] Rancang solusi kembali ke versi sebelumnya pada proses penyebaran.


---

## Lihat juga:
- [yosriady/api-development-tools](https://github.com/yosriady/api-development-tools) - Kumpulan sumber yang berguna untuk membangun API RESTful HTTP+JSON.


---

# Kontribusi
Silahkan berkontribusi dengan cara menduplikasi repositori ini, lakukan perubahan, dan kirimkan PR. Jika ada pertanyaan silakan kirim email ke `team@shieldfy.io`.
