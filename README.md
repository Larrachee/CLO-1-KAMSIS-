import os

# Fungsi enkripsi dasar
def enkripsi(byte_data, key):
    # 1. Substitusi XOR
    hasil = byte_data ^ key
    
    # 2. Rotasi kiri 3 bit
    hasil = ((hasil << 3) | (hasil >> 5)) & 255
    
    # 3. Permutasi bit (dibalik)
    biner = bin(hasil)[2:].zfill(8)
    balik = biner[::-1]
    return int(balik, 2)

# Fungsi dekripsi dasar
def dekripsi(byte_data, key):
    # 1. Balik permutasi (kebalikan)
    biner = bin(byte_data)[2:].zfill(8)
    balik = biner[::-1]
    hasil = int(balik, 2)
    
    # 2. Rotasi kanan 3 bit
    hasil = ((hasil >> 3) | (hasil << 5)) & 255
    
    # 3. Substitusi XOR
    return hasil ^ key

print("=== TUGAS CLO 1 KEAMANAN SISTEM ===")
kunci_input = input("Masukin Kunci: ")

# Bikin kunci jadi angka 0-255
kunci = 0
for huruf in kunci_input:
    kunci += ord(huruf)
kunci = kunci % 256

while True:
    print("\nMenu:")
    print("1. Enkripsi Teks (Nama & NIM)")
    print("2. Enkripsi/Dekripsi File")
    print("3. Keluar")
    pilih = input("Pilih (1/2/3): ")

    if pilih == '1':
        teks = input("Masukkan Nama & NIM: ")
        hasil_teks = ""
        for t in teks:
            angka = ord(t)
            enk = enkripsi(angka, kunci)
            hasil_teks += chr(enk)
            
        print("Hasil (Hex):", hasil_teks.encode('latin-1').hex())

    elif pilih == '2':
        nama_file = input("Nama file (contoh: foto.jpg): ")
        if not os.path.exists(nama_file):
            print("File gak ada!")
            continue

        mode = input("Pilih mode (E untuk Enkripsi / D untuk Dekripsi): ")
        
        if mode.lower() == 'e':
            out = "terkunci.enc"
        else:
            out = "pulih_kembali.jpg"

        # Buka dan baca file
        f1 = open(nama_file, 'rb')
        isi_file = f1.read()
        f1.close()

        hasil_file = bytearray()
        for byte in isi_file:
            if mode.lower() == 'e':
                hasil_file.append(enkripsi(byte, kunci))
            else:
                hasil_file.append(dekripsi(byte, kunci))

        # Simpan file baru
        f2 = open(out, 'wb')
        f2.write(hasil_file)
        f2.close()
        
        print("Selesai disimpan di:", out)

    elif pilih == '3':
        break
