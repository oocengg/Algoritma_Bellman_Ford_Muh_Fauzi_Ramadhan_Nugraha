# Algoritma Bellman-Ford: analisis, implementasi, dan eksperimen

Tugas Analisis Algoritma No 7, mata kuliah Algoritma Komputasi, Magister Teknik Informatika, Universitas Hasanuddin.
Muh Fauzi Ramadhan Nugraha, D082261017.

Bellman-Ford menghitung jarak termurah dari satu simpul sumber ke semua simpul lain pada graf berarah yang bobotnya boleh negatif, dan melaporkan siklus negatif yang bisa dicapai dari sumber. Repositori ini berisi laporan, kode, dan halaman presentasi.

## Struktur

```
code/
  Bellman_Ford_Sederhana.ipynb    inti Bellman-Ford tanpa pustaka, tiga kasus kecil, dan studi kasus
  Bellman_Ford_Eksperimen.ipynb   inti yang sama, uji silang dengan scipy, eksperimen waktu, dan grafik
  hasil_eksperimen.csv            data mentah eksperimen (rata-rata dan simpangan baku dari 5 ulangan)
  grafik_waktu_tiga_kasus.png     waktu pada kasus terburuk, rata-rata, dan terbaik beserta kurva teori
  grafik_pembanding.png           pembandingan dengan versi tanpa henti dini, scipy, dan Dijkstra
  requirements.txt                pustaka untuk notebook eksperimen
docs/
  Algoritma_Bellman_Ford_Muh_Fauzi_Ramadhan_Nugraha.pdf   laporan (format konferensi IEEE)
presentation/
  Presentasi_Bellman_Ford.html    halaman presentasi interaktif
```

## Menjalankan

**Notebook sederhana** tidak memakai pustaka apa pun (tidak ada `import`). Cukup Python 3.10 atau lebih baru dan Jupyter. Buka notebook, lalu pilih Kernel, Restart and Run All. Setiap kasus dicek dengan `assert` terhadap hasil hitung tangan.

**Notebook eksperimen** memerlukan numpy, matplotlib, dan scipy. Pasang di kernel yang sama dengan yang dipakai Jupyter:

```bash
pip install -r code/requirements.txt
```

Pustaka hanya dipakai di luar inti algoritma: numpy dan matplotlib untuk statistik dan grafik, dan scipy sebagai kunci jawaban pada uji silang. Seluruh eksperimen berjalan sekitar 13 menit pada Apple M2 Pro. Saat dijalankan, notebook ini menulis ulang `hasil_eksperimen.csv`, `grafik_waktu_tiga_kasus.png`, dan `grafik_pembanding.png` di folder `code/`. Seed acak tetap (2026), jadi graf yang dibangkitkan selalu sama, sedangkan waktu dinding sedikit berbeda pada setiap jalan.

**Halaman presentasi** cukup dibuka di peramban. Berkasnya mandiri, tanpa pustaka luar. Isinya:

1. Masalah
2. Ide dan pseudocode
3. Perbandingan dengan Dijkstra
4. Tiga kasus kecil, dijalankan langkah demi langkah di samping kode, dengan isi variabel dan keluaran program
5. Studi kasus Sulawesi Selatan
6. Kelebihan dan kekurangan
7. Notasi Big O
8. Penerapan di dunia nyata
9. Kesimpulan

Tombol "Penjelasan" menampilkan atau menyembunyikan kotak penjelasan, dan tombol ← → berpindah antarbagian.

## Kasus uji

| Kasus | Yang ditunjukkan | Hasil |
|---|---|---|
| Rute kurir 4 titik | Bobot negatif membuat Dijkstra salah | Bellman-Ford: Toko A 2, Pelanggan 4. Dijkstra: 4 dan 6 |
| Segitiga tukar uang | Siklus negatif (arbitrase) terdeteksi | Kurs keliling 1,0368, bobot −36, siklus dilaporkan |
| Rantai 4 titik | Urutan sisi terburuk memakai semua \|V\| − 1 putaran | Daftar terbalik 12 pemeriksaan, daftar searah 9, jarak sama |
| Distribusi barang Sulawesi Selatan (8 kota) | Bobot negatif pada graf lebih besar | Sidrap 175 dan Enrekang 245. Dijkstra: 200 dan 270 |

Selain itu, 1.000 graf acak kecil dicocokkan dengan scipy: 0 selisih.

## Hasil eksperimen

| Temuan | Angka |
|---|---|
| Pangkat pertumbuhan k pada waktu ≈ c·\|V\|^k, kasus terburuk (teori 2) | 1,94 |
| Pangkat pertumbuhan, kasus terbaik (teori 1) | 1,03 |
| Jumlah putaran pada graf acak, 1.000 → 300.000 simpul | 8,2 → 15,8 (sebanding log \|V\|) |
| Henti dini pada graf acak 4.000 simpul | 353× lebih cepat |
| Dijkstra pada bobot nonnegatif | 3,7–8,0× lebih cepat, tetapi salah bila ada bobot negatif |

## Catatan

- Angka di laporan diambil dari `hasil_eksperimen.csv`. Keluaran yang tersimpan di notebook eksperimen berasal dari jalan sebelumnya, jadi waktu yang tercetak bisa sedikit berbeda. Jumlah putaran dan seluruh jarak selalu sama karena seed tetap.
- `grafik_pembanding.png` adalah keluaran notebook dan masih memuat panel scipy. Laporan memakai versi dua panel, karena scipy hanya dipakai sebagai kunci jawaban, bukan pembanding waktu.
- Kasus kurir, segitiga tukar uang, dan studi kasus Sulawesi Selatan memakai angka rekaan.
- Kode dan naskah disusun dengan bantuan asisten AI Claude (Anthropic).
