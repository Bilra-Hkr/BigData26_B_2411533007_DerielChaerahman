# Kamus Data — tabel_analitik

| Kolom | Tipe | Satuan | Sumber | Cara Hitung | Penanganan Null |
|---|---|---|---|---|---|
| borough_naik | string | — | taxi_zone_lookup.csv | join PULocationID | baris tanpa match dieksklusi |
| jam_mulai | datetime | jam | tpep_pickup_datetime | floor('h') | — |
| jumlah_perjalanan | int | perjalanan | trips | COUNT per grup | — |
| rata_tarif_per_mil | float | USD/mil | trips | median(total_amount/trip_distance) | grup < 30 perjalanan dibuang |
| rata_kecepatan | float | mph | trips | median(trip_distance/(durasi_menit/60)) | sama |
| suhu_c | float | °C | Open-Meteo API | first() per jam-borough | diisi 0 bila API tidak ada data |
| hujan | int8 | 0/1 | Open-Meteo API | max(hujan_mm > 0.1) per grup | diisi 0 |

## Batasan
1. Median dipakai menggantikan mean karena distribusi tarif per mil sangat miring ke kanan.
2. Cuaca diukur di satu titik koordinat (40.71°N, 74.00°W) untuk seluruh kota — variasi spasial tidak tertangkap.
3. Korelasi tarif–hujan tidak membuktikan kausalitas; jam sibuk bisa bersamaan dengan hujan.
4. Kelompok < 30 perjalanan dibuang — borough kecil (Staten Island) bisa kehilangan banyak jam.
