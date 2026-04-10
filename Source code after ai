import os

class KriptoEngine:
    def __init__(self, key):
        # Kunci harus 8-bit (0-255)
        self.key = sum(ord(c) for c in str(key)) % 256

    # --- METODE KRIPTOGRAFI DASAR ---
    def _substitusi_xor(self, b):
        return b ^ self.key # Metode 1

    def _rotasi_bit(self, b, arah='kiri'):
        if arah == 'kiri':
            return ((b << 3) | (b >> 5)) & 0xFF # Metode 21
        return ((b >> 3) | (b << 5)) & 0xFF

    def _permutasi_bit(self, b):
        # Membalik urutan bit (bit 0-7 dibalik) 
        return int('{:08b}'.format(b)[::-1], 2)

    # --- PROSES CORE ---
    def enkripsi_byte(self, b):
        b = self._substitusi_xor(b)
        b = self._rotasi_bit(b, 'kiri')
        b = self._permutasi_bit(b)
        return b

    def dekripsi_byte(self, b):
        b = self._permutasi_bit(b) # Kebalikan permutasi balik adalah dirinya sendiri
        b = self._rotasi_bit(b, 'kanan')
        b = self._substitusi_xor(b)
        return b

# --- ANTARMUKA APLIKASI (CLI) ---
def main():
    print("=== APLIKASI PENYANDIAN DATA - PRODI S1 TEKNIK KOMPUTER ===")
    print("Target: Projek CLO 1 - Keamanan Sistem [cite: 2, 4]")
    
    key_input = input("Masukkan Kunci (Angka/Teks): ")
    engine = KriptoEngine(key_input)
    
    while True:
        print("\nMenu Utama:")
        print("1. Enkripsi Teks (Nama & NIM)")
        print("2. Enkripsi/Dekripsi File (Gambar/Video/Lainnya)")
        print("3. Keluar")
        
        pilihan = input("Pilih menu (1/2/3): ")
        
        if pilihan == '1':
            teks = input("Masukkan Nama & NIM: ") # Syarat minimal [cite: 25]
            hasil = "".join(chr(engine.enkripsi_byte(ord(c))) for c in teks)
            print(f"Hasil Enkripsi (Ciphertext): {hasil.encode('latin-1').hex()}")
            
        elif pilihan == '2':
            path = input("Masukkan path file (contoh: foto.jpg): ")
            if not os.path.exists(path):
                print("File tidak ditemukan!")
                continue
                
            mode = input("Pilih mode (E untuk Enkripsi / D untuk Dekripsi): ").lower()
            
            # Ganti baris output_path lama dengan ini:
            output_path = "terkunci.enc" if mode == 'e' else "pulih_kembali.jpg"
            
            with open(path, 'rb') as f_in, open(output_path, 'wb') as f_out:
                
                data = f_in.read()
                hasil = bytearray()
                for b in data:
                    if mode == 'e':
                        hasil.append(engine.enkripsi_byte(b))
                    else:
                        hasil.append(engine.dekripsi_byte(b))
                f_out.write(hasil)
            print(f"Selesai! Hasil disimpan di: {output_path}")
            
        elif pilihan == '3':
            break

if __name__ == "__main__":
    main()
