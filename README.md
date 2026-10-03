# [Judul Proyek] — Transfer Learning untuk Robot

**Kelompok:** [nama anggota] | **Topik:** [sampah / buah / rintangan / rambu]

## 1. Dokumen Desain Awal
- **Masalah & tujuan:** [apa yang harus dikenali robot dan untuk apa]
- **Kelas:** [daftar kelas + definisi singkat tiap kelas]
- **Perangkat target:** [Raspberry Pi / Jetson / laptop] → **Model dipilih:** [nama] karena [alasan: parameter, GFLOPs, akurasi ImageNet]
- **Pengambilan data:** [kamera, jarak, pencahayaan, jumlah per kelas, sumber]
- **Preprocessing:** undistort → resize 224×224 → BGR→RGB → normalisasi (mean 0,485/0,456/0,406; std 0,229/0,224/0,225), identik saat training & deployment
- **Metrik sukses:** [mis. akurasi test ≥ 90%, latensi ≤ 100 ms]

## 2. Dataset
`dataset_raw/` + `metadata.csv` (kolom: filename, label, sumber, split). Jumlah per kelas: [isi dari output make_metadata.py]

## 3. Cara menjalankan
```bash
pip install -r requirements.txt
python make_metadata.py --source "foto sendiri"
python train.py --model mobilenet_v3_small --epochs 15
python benchmark_latency.py --ckpt results/model_fine_tuning.pt
```

## 4. Hasil 3 Mode
(salin dari `results/hasil_3_mode.csv`)

| Mode | Trainable params | Best val acc | Test acc | Waktu latih (s) |
|---|---|---|---|---|
| Scratch | | | | |
| Feature extraction | | | | |
| Fine-tuning | | | | |

![Akurasi per epoch](results/akurasi_per_epoch.png)

## 5. Latensi Model Terpilih
(salin dari `results/latensi.csv`) — perangkat: [..], mean: [..] ms, p95: [..] ms, FPS: [..]

## 6. Analisis Singkat
- Mode mana yang terbaik dan mengapa (bandingkan kurva epoch & test acc)?
- Apakah scratch kalah karena data hanya ±50 citra/kelas?
- Kelas mana yang sering salah, dan apa penyebab (cahaya, sudut, mirip)?
- Apakah latensi memenuhi kebutuhan real-time robot? Apa trade-off akurasi vs kecepatan?
