# ==========================================
# PENDAFTARAN AHLI KELAB ROBOTIK
# ==========================================


nama = input("Masukkan Nama: ")
umur = input("Masukkan Umur: ")
program = input("Masukkan Program Pengajian: ")
semester = input("Masukkan Semester: ")
robot_pilihan = input("Masukkan Nama Robot Pilihan: ")
tahun_pengajian = input("Masukkan Tahun Pengajian: ")
minat_ai = input("Adakah anda berminat dalam AI? (Ya/Tidak): ")


if minat_ai == "Ya" or minat_ai == "ya":
    status = "Diterima (Sila sertai Unit AI)"
else:
    status = "Diterima (Sila sertai Unit Robotik Biasa)"


print("\n==========================================")
print("       PENDAFTARAN AHLI KELAB ROBOTIK     ")
print("==========================================")
print(f"Nama            : {nama}")
print(f"Umur            : {umur}")
print(f"Program         : {program}")
print(f"Semester        : {semester}")
print(f"Robot Pilihan   : {robot_pilihan}")
print(f"Tahun Pengajian : {tahun_pengajian}")
print(f"Minat dalam AI  : {minat_ai}")
print(f"Status KV-TRON  : {status}")


print("\n==========================================")
print("Jenis Data")
print("==========================================")
print(f"Nama            : {type(nama)}")
print(f"Umur            : {type(umur)}")
print(f"Program         : {type(program)}")
print(f"Semester        : {type(semester)}")
print(f"Robot Pilihan   : {type(robot_pilihan)}")
print(f"Tahun Pengajian : {type(tahun_pengajian)}")
print(f"Minat dalam AI  : {type(minat_ai)}")


<img width="385" height="403" alt="image" src="https://github.com/user-attachments/assets/a2d252be-3f1c-4829-9b55-1a8977a9abf9" />



