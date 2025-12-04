# Menggunakan base image PHP 8.2 FPM (FastCGI Process Manager)
FROM php:8.2-fpm

# 1. Install System Dependencies
# Kita butuh tools linux dasar dan library untuk gambar/zip
RUN apt-get update && apt-get install -y \
    git \
    curl \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    zip \
    unzip

# 2. Bersihkan Cache
# Agar ukuran image tetap kecil
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# 3. Install Ekstensi PHP
# Laravel butuh pdo_mysql (database), mbstring (string), gd (gambar), dll.
RUN docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd

# 4. Install Composer
# Mengambil composer terbaru dari image official composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# 5. Set Working Directory
WORKDIR /var/www

# 6. Copy Source Code
# Menyalin semua file dari laptop ke dalam container
COPY . /var/www

# 7. Install Dependensi Laravel (Production Mode)
# --no-dev: Library testing tidak ikut diinstall
# --optimize-autoloader: Supaya loading class lebih cepat
RUN composer install --no-dev --optimize-autoloader

# 8. Atur Permission (SANGAT PENTING)
# Folder storage dan cache harus bisa ditulisi oleh www-data (user default Nginx/PHP)
RUN chown -R www-data:www-data /var/www/storage /var/www/bootstrap/cache