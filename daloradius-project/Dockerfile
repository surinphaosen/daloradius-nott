FROM php:7.4-apache

# 1. ติดตั้ง Dependencies (รวมเป็นชั้นเดียวเพื่อลดขนาด)
RUN apt-get update && apt-get install -y \
    git \
    unzip \
    libpng-dev \
    libmariadb-dev \
    libzip-dev \
    && docker-php-ext-install gd mysqli pdo_mysql zip \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# 2. Download daloRADIUS
WORKDIR /var/www/html
RUN git clone https://github.com/lirantal/daloradius.git . 

# 3. แก้ไขจุดที่มีปัญหา: ค้นหาไฟล์ sample แล้ว copy ไปเป็นไฟล์จริง
# คำสั่ง find จะช่วยหาไฟล์ daloradius.conf.php.sample ไม่ว่ามันจะอยู่ใน library/ หรือ library/config/
RUN SAMPLE_FILE=$(find . -name daloradius.conf.php.sample | head -n 1) && \
    cp "$SAMPLE_FILE" library/daloradius.conf.php && \
    cp "$SAMPLE_FILE" library/config/daloradius.conf.php || true && \
    chown -R www-data:www-data /var/www/html

# 4. Enable Apache Rewrite
RUN a2enmod rewrite

# 5. ตั้งค่า Environment Variable (Default values)
ENV DB_HOST=db
ENV DB_USER=radius
ENV DB_PASS=radpass
ENV DB_NAME=radius

EXPOSE 80