Prerequisite:

Pastikan Docker Engine menyala.
Buat struktur folder project dengan bentuk sebagai berikut:
conf.d
db
nginx
php
src
docker-compose.yml

untuk detail isian pada struktur folder dapat dilihat pada repository saya di
https://github.com/panjivj/uas-b-pemrograman.git

Development:
untuk mulai development ikuti langkah-langkah berikut:

1. Kita build docker container terlebih dahulu jalankan:
   docker compose up -d --build

2. Cek daftar nama project dengan ketik:
   docker ps

3. kemudian kita masuk ke container project:
   docker exec -it fill

4. Lanjut Create project
   composer create-project laravel/laravel .

5. Mulai install filament framework
   composer require livewire/livewire
   composer require filament/filament:"^3.1" -W

6. Setting environment project

   pertama kita ambil template environtment terlebih dahulu.
   mv .env.example .env

   selanjutnya kita perlu mengupdate user password untuk mengakses pada database pada file .env
   perhatikan pada bagian baris kode berikut ini:

   DB_CONNECTION=mysql
   DB_HOST=db
   DB_PORT=3306
   DB_DATABASE=cr001
   DB_USERNAME=root
   DB_PASSWORD=p455w0rd

   selanjutnya update data tersebut dan samakan dengan user akses pada file docker-compose.yml agar user akses sesuai dengan user akses pada container database yang dibuat.

   selanjutnya generate key terlebih dahulu dengan ketik:
   php artisan key:generate

   Untuk keperluan akses folder pada file-file gambar yang akan kita upload,dll maka kita perlu seting aksesnya sebagai berikut:

   Buat folder dan hubungkan folder:
   mkdir storage
   php artisan storage:link

   Seting hak akses system:
   chmod 777 -R storage/\*

7. Mulai lakukan migrasi data awal dengan ketik:
   php artisan migrate

8. Selanjutnya kita bisa mulai install framework dashboard filament dengan ketik:
   php artisan filament:install --panels

9. Slanjutnya agar kita bisa login ke panel dashboard ketika ketik perintah berikut untuk membuat user:
   php artisan make:filament-user

   masukan username kamu misalnya: admin
   masukan email kamu misalnya: admin@amdin.com
   masukan password kamu misalnya: 123456

Kamu bisa masuk ke panel pada url:
http://localhost/admin/login

10. Selanjutnya masih pada layar shell pada container project docker, kita bisa mulai membuat model terlebih dahulu untuk bagian Category, Tag, Produk. ketik syntak berikut:
    php artisan make:model Category -m
    php artisan make:model Tag -m
    php artisan make:model Product -m

Maka secara otomatis system akan melakukan generate file pada folder src/app/Models berupa 3 file yaitu:
Category.php
Product.php
Tag.php

Sesuaikan isi pada file tersebut sesuai dengan file kode saya yang ada di github.

11. Kemudian kita buat kode resourcenya untuk keperluan form editor, dll:
    php artisan make:filament-resource Category --generate
    php artisan make:filament-resource Tag --generate
    php artisan make:filament-resource Product --generate

Maka secara otomatis system akan melakukan generate kode pada folder src/app/Filament/Resources

Difolder tersebut secara otomatis akan berisi folder sbb:
CategoryResource
ProductResource
TagResource

Sesuaikan isi pada file tersebut sesuai dengan file kode saya yang ada di github.

12. Edit tampilan group menu.
    Untuk membuat tampilan group menu caranya sebagai berikut:

# Group Shop

Caranya masuk ke folder src/app/Filament/Resources/
edit pada file ProductResource.php

tambahkan kode berikut:
protected static ?string $navigationGroup = 'Shop';

# Group Setting Product

Caranya masuk ke folder src/app/Filament/Resources/
edit pada file CategoryResource.php dan TagResource.php

tambahkan kode berikut:
protected static ?string $navigationGroup = 'Setting Product';

# Group Management User

Caranya masuk ke folder src/app/Filament/Resources/
edit pada file ProductResource.php

tambahkan kode berikut:
protected static ?string $navigationGroup = 'Management User';

jika belum ada file tersebut kamu dapat melakukan generate resource dengan petuntuk pada nomor 11

---
