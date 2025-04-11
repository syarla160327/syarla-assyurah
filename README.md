nama : syarla assyurah
nim  : 09030182428002

1.Perintah ps -au dan Analisis Proses
a.Nama-nama proses yang bukan root
Dari hasil ps -au, proses yang bukan milik root misalnya:
syarla
student (tergantung user yang login)
b.PID dan COMMAND dari proses yang paling banyak menggunakan CPU
Semua proses menunjukkan %CPU = 0.0. Maka dipilih proses yang aktif (R+):
PID: 427
COMMAND: ps -au
![Screenshot 2025-04-11 144133](https://github.com/user-attachments/assets/0061da57-8d98-48e2-ab8f-a4e0e6b2fabd)
c.Buyut proses dan PID dari proses tersebut
Tracing dari proses ps -au:
![Screenshot 2025-04-11 144413](https://github.com/user-attachments/assets/e9d4b996-e652-45ad-80ec-a0bc464fb155)
Jadi, buyut prosesnya adalah:
PID: 1
COMMAND: /init
d. Beberapa proses daemon
Contoh proses daemon dari ps -au:
systemd
cron
rsyslogd
gvfsd (dll, tergantung sistem)
e.Urutan proses dari PID terbesar hingga PPID = 1
Contoh tracing dari PID 610:
![Screenshot 2025-04-11 153703](https://github.com/user-attachments/assets/e4a22fec-ac2d-4841-89f1-5ea98b23ceae)
PID terbesar: 610 (sh)
Urutan lengkap sampai ke induk utama (PPID = 1) ditelusuri dengan ps -fp [PID]

2.Program prog.sh dengan Trap
![Screenshot 2025-04-11 154334](https://github.com/user-attachments/assets/17fc0a13-1388-49c2-b24f-532051d3e344)
Script
#!/bin/sh
trap "echo Hello Goodbye ; exit 0" 1 2 3 15
echo "Program berjalan …"
while :
do
  echo "X"
  sleep 20
done
Program berhasil menangkap sinyal 1, 2, 3, dan 15
Setiap sinyal yang dikirim menampilkan: "Hello Goodbye"
Proses langsung berhenti setelah sinyal diterima
Fungsi trap bekerja sesuai yang diharapkan

3.Program myjob.sh dengan Auto-Hapus File
![Screenshot 2025-04-11 155321](https://github.com/user-attachments/assets/cfcdef8f-038c-414c-9b9a-48bd86cffe04)
Scrapt
#!/bin/sh
trap "rm -f berkas hasil; echo 'File dihapus karena proses dihentikan'; exit 0" 1 2 3 15
i=1
while :
do
  find / -print > berkas
  sort berkas -o hasil
  echo "Proses selesai pada $(date)" >> proses.log
  sleep 60
done
![Screenshot 2025-04-11 155553](https://github.com/user-attachments/assets/812dd21c-bac6-446c-a762-49484a9334f1)
aat proses dihentikan dengan kill -15 783, muncul:
![Screenshot 2025-04-11 155700](https://github.com/user-attachments/assets/51ea9e1b-e8fc-41aa-87c6-4e98acf1434f)
![Screenshot 2025-04-11 155841](https://github.com/user-attachments/assets/17ea37f2-e1bf-464e-a1d8-7115febb3353)
File berkas dan hasil otomatis terhapus
Fungsi trap berjalan sukses




