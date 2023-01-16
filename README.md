# pratikum9

# Membuat Menu Utama

Pertama kita membutuhkan tampilan utama dari aplikasi ini. Kita ingin menampilkan menu-menu atau fitur yang ada di dalam aplikasi.

Mari kita langsung buat..

Silahkan buat program baru bernama app_csv.py, kemudian isi dengan kode berikut:

![Screenshot (280)](https://user-images.githubusercontent.com/115479895/212718913-80def77a-08fd-4c09-b83a-23f1ad7a6083.png)

Modul csv kita butuhkan untuk membaca dan menulis file CSV.

Lalu modul os kita butuhkan untuk melakukan clear screen.

Kita juga menyiapkan variabel global bernama csv_filename untuk menentukan file CSV yang akan digunakan.

Untuk saat ini, variabel ini belum kita pakai.. karena kita belum membuat fungsi CRUDS.

Selanjutnya coba perhatikan fungsi clear_screen()

def clear_screen():

    os.system('cls' if os.name == 'nt' else 'clear')
    

Fungsi ini akan kita gunakan untuk membersihkan layar. Fungsi ini sebenarnya akan menjalankan perintah cls (jika di Windows) dan clear (jika di Linux dan Unix).

Berikutnya kita membuat fungsi show_menu() yang akan menampilkan daftar menu dan menjalankan fungsi tertentu sesuai dengan menu yang dipilih oleh user.

Tereakhir kita membuat fungsi back_to_menu() untuk kembali ke menu utama.

Aplikasi ini belum bisa dijalankan, karena kita belum membuat fungsi-fungsi yang lain seperti show_contact(), create_contact(), edit_contact(), delete_contact(), dan search_contact().

Tapi kalau kamu penasaran hasilnya seperti apa..

..ini adalah hasil akhirnya nanti:

![Screenshot (287)](https://user-images.githubusercontent.com/115479895/212719674-db999f60-5783-48f3-ba5c-06e3326d3f3f.png)

# Maka sekarang kode lengkap programnya akan menjadi seperti ini:

import csv

import os

csv_filename = 'contacts.csv'

def clear_screen():

    os.system('cls' if os.name == 'nt' else 'clear')

def show_menu():

    clear_screen()
    
    print("=== APLIKASI KONTAK ===")
    
    print("[1] Lihat Daftar Kotak")
    
    print("[2] Buat Kontak Baru")
    
    print("[3] Edit Kontak")
    
    print("[4] Hapus Kontak")
    
    print("[5] Cari Kontak")
    
    print("[0] Exit")
    
    print("------------------------")
    
    selected_menu = input("Pilih menu> ")
    
    if(selected_menu == "1"):
    
        show_contact()
        
    elif(selected_menu == "2"):
    
        create_contact()
        
    elif(selected_menu == "3"):
    
        edit_contact()
        
    elif(selected_menu == "4"):
    
        delete_contact()
        
    elif(selected_menu == "5"):
    
        search_contact()
        
    elif(selected_menu == "0"):
    
        exit()
        
    else:
    
        print("Kamu memilih menu yang salah!")
        
        back_to_menu()

def back_to_menu():

    print("\n")
    
    input("Tekan Enter untuk kembali...")
    
    show_menu()


def show_contact():

    clear_screen()
    
    contacts = []
    
    with open(csv_filename) as csv_file:
    
        csv_reader = csv.reader(csv_file, delimiter=",")
        
        for row in csv_reader:
        
            contacts.append(row)

    if (len(contacts) > 0):
        labels = contacts.pop(0)
        print(f"{labels[0]} \t {labels[1]} \t\t {labels[2]}")
        print("-"*34)
        for data in contacts:
            print(f'{data[0]} \t {data[1]} \t {data[2]}')
    else:
        print("Tidak ada data!")
    back_to_menu()


def create_contact():

    clear_screen()
    
    with open(csv_filename, mode='a') as csv_file:
    
        fieldnames = ['NO', 'NAMA', 'TELEPON']
        
        writer = csv.DictWriter(csv_file, fieldnames=fieldnames)
        
        no = input("No urut: ")
        
        nama = input("Nama lengkap: ")
        
        telepon = input("No. Telepon: ")

        writer.writerow({'NO': no, 'NAMA': nama, 'TELEPON': telepon})
        
        print("Berhasil disimpan!")

    back_to_menu()


def search_contact():

    clear_screen()
    
    contacts = []

    with open(csv_filename, mode="r") as csv_file:
        csv_reader = csv.DictReader(csv_file)
        for row in csv_reader:
            contacts.append(row)

    no = input("Cari berdasrakan nomer urut> ")

    data_found = []

    # mencari contact
    indeks = 0
    for data in contacts:
        if (data['NO'] == no):
            data_found = contacts[indeks]
            
        indeks = indeks + 1

    if len(data_found) > 0:
        print("DATA DITEMUKAN: ")
        print(f"Nama: {data_found['NAMA']}")
        print(f"Telepon: {data_found['TELEPON']}")
    else:
        print("Tidak ada data ditemukan")
    back_to_menu()
    


def edit_contact():

    clear_screen()
    
    contacts = []

    with open(csv_filename, mode="r") as csv_file:
    
        csv_reader = csv.DictReader(csv_file)
        
        for row in csv_reader:
        
            contacts.append(row)

    print("NO \t NAMA \t\t TELEPON")
    
    print("-" * 32)

    for data in contacts:
    
        print(f"{data['NO']} \t {data['NAMA']} \t {data['TELEPON']}")

    print("-----------------------")
    
    no = input("Pilih nomer kontak> ")
    
    nama = input("nama baru: ")
    
    telepon = input("nomer telepon baru: ")

    # mencari contact dan mengubah datanya
    
    # dengan data yang baru
    indeks = 0
    
    for data in contacts:
    
        if (data['NO'] == no):
        
            contacts[indeks]['NAMA'] = nama
            
            contacts[indeks]['TELEPON'] = telepon
            
        indeks = indeks + 1

    # Menulis data baru ke file CSV (tulis ulang)
    
    with open(csv_filename, mode="w") as csv_file:
    
        fieldnames = ['NO', 'NAMA', 'TELEPON']
        
        writer = csv.DictWriter(csv_file, fieldnames=fieldnames)
        
        writer.writeheader()
        
        for new_data in contacts:
        
            writer.writerow({'NO': new_data['NO'], 'NAMA': new_data['NAMA'], 'TELEPON': new_data['TELEPON']}) 

    back_to_menu()



def delete_contact():

    clear_screen()
    
    contacts = []

    with open(csv_filename, mode="r") as csv_file:
    
        csv_reader = csv.DictReader(csv_file)
        
        for row in csv_reader:
        
            contacts.append(row)

    print("NO \t NAMA \t\t TELEPON")
    
    print("-" * 32)

    for data in contacts:
    
        print(f"{data['NO']} \t {data['NAMA']} \t {data['TELEPON']}")

    print("-----------------------")
    
    no = input("Hapus nomer> ")

    # mencari contact dan mengubah datanya
    
    # dengan data yang baru
    
    indeks = 0
    
    for data in contacts:
    
        if (data['NO'] == no):
        
            contacts.remove(contacts[indeks])
            
        indeks = indeks + 1

    # Menulis data baru ke file CSV (tulis ulang)
    
    with open(csv_filename, mode="w") as csv_file:
    
        fieldnames = ['NO', 'NAMA', 'TELEPON']
        
        writer = csv.DictWriter(csv_file, fieldnames=fieldnames)
        
        writer.writeheader()
        
        for new_data in contacts:
        
            writer.writerow({'NO': new_data['NO'], 'NAMA': new_data['NAMA'], 'TELEPON': new_data['TELEPON']}) 

    print("Data sudah terhapus")
    
    back_to_menu()

if __name__ == "__main__":

    while True:
    
        show_menu()
        
        
        
 # Tambah data       
![Screenshot (293)](https://user-images.githubusercontent.com/115479895/212724902-9c76c5b0-094e-4b07-99d5-a15bd7b77bd0.png)



# Edit Kontak

![Screenshot (294)](https://user-images.githubusercontent.com/115479895/212725118-b75bfb20-e717-46f7-8be0-0105e8b3195c.png)

# Cari Kontak

![Screenshot (295)](https://user-images.githubusercontent.com/115479895/212725233-7788213a-1d58-48bf-925c-9594d70a5e4f.png)


# Exit

![Screenshot (297)](https://user-images.githubusercontent.com/115479895/212725408-b5a6b3e0-a1bd-4e84-83b9-c88fb4567a2e.png)





        

import csv

with open('contacts.csv') as csv_file:

    csv_reader = csv.reader(csv_file, delimiter=",")
    
    print(csv_reader)
    
    for data in csv_reader:
    
            print(data)
            
            
import csv

contacts = []

with open('contacts.csv') as csv_file:

    csv_reader = csv.reader(csv_file, delimiter=",")
    
    for row in csv_reader:
    
        contacts.append(row)


labels = contacts.pop(0)

#print(labels)

#print(contacts)

print(f'{labels[0]} \t {labels[1]} \t\t {labels[2]}')

print("-"*34)

for data in contacts:

    print(f'{data[0]} \t {data[1]} \t {data[2]}')
    
    
import csv

contacts = []

with open('contacts.csv') as csv_file:

    csv_reader = csv.DictReader(csv_file)
    
    for row in csv_reader:
    
        contacts.append(row)



print("NO \t NAMA \t\t TELEPON")

print("-" * 32)

for data in contacts:

    print(f"{data['NO']} \t {data['NAMA']} \t {data['TELEPON']}")
    
    
import csv

with open('contacts.csv', mode='a') as csv_file:

    # menentukan label
    
    fieldnames = ['NO', 'NAMA', 'TELEPON']

    # membuat objek writer
    
    writer = csv.DictWriter(csv_file, fieldnames=fieldnames)

    # menulis baris ke file CSV
    
    writer.writeheader()
    
    writer.writerow({'NO': '10', 'NAMA': 'Via Vallen', 'TELEPON': '02109999'})
    
    writer.writerow({'NO': '11', 'NAMA': 'M. Andi', 'TELEPON': '02148488888'})

print("Writing Done!")

import csv

contacts = []

with open('contacts.csv', mode='a') as csv_file:

    # membuat objek writer
    
    writer = csv.writer(csv_file, delimiter=',', quotechar='"', quoting=csv.QUOTE_MINIMAL)
    
    # menulis baris ke file CSV
    
    writer.writerow(["5", "Dian", "021100022"])
    
    writer.writerow(["6", "Meli", "0214444432"])
    

print("Writing Done!")
