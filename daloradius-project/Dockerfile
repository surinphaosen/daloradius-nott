FROM php:7.4-apache

# 1. ติดตั้ง Dependencies
RUN apt-get update && apt-get install -y \
    git unzip libpng-dev libmariadb-dev libzip-dev \
    && docker-php-ext-install gd mysqli pdo_mysql zip \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# 2. เตรียมพื้นที่
WORKDIR /var/www/html

# 3. Clone และจัดการไฟล์ (ย้ายออกมาจากโฟลเดอร์ที่ Git สร้าง)
RUN git clone https://github.com/lirantal/daloradius.git /tmp/daloradius \
    && cp -rf /tmp/daloradius/* . \
    && cp -rf /tmp/daloradius/.[!.]* . \
    && rm -rf /tmp/daloradius

# 4. ตั้งค่า Config (ค้นหาไฟล์ sample ให้เจอไม่ว่าอยู่ไหน)
RUN SAMPLE_FILE=$(find . -name daloradius.conf.php.sample | head -n 1) && \
    mkdir -p library/config && \
    cp "$SAMPLE_FILE" library/daloradius.conf.php && \
    cp "$SAMPLE_FILE" library/config/daloradius.conf.php && \
    chown -R www-data:www-data /var/www/html && \
    chmod -R 755 /var/www/html

# 5. Enable Apache Rewrite
RUN a2enmod rewrite

EXPOSE 80