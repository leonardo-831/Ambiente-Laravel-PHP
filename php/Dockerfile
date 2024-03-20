#imagem puxada
FROM php:8.2-fpm

# diretório raiz do container
WORKDIR /var/www/html

#atualiza e instala pacotes e dependencias que podemos usar
RUN apt-get update && apt-get install -y \
    build-essential \
    libpng-dev \
    libjpeg62-turbo-dev \
    libfreetype-dev \
    locales \
    zip \
    jpegoptim optipng pngquant gifsicle \
    joe \
    unzip \
    git \
    curl \
    libzip-dev

# Limpa o cache do gerenciador de pacotes
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Baixa e configura algumas extensões do php
RUN docker-php-ext-configure gd --enable-gd --with-jpeg --with-freetype
RUN docker-php-ext-install -j$(nproc) gd
RUN docker-php-ext-install pdo_mysql zip exif pcntl

# Baixa o composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Cria um grupo de usuário chamado leonardo com id 1000
RUN groupadd -g 1000 leonardo
# Cria um usuário chamado leonardo com id 1000 e adiciona ao grupo leonardo
RUN useradd -u 1000 -ms /bin/bash -g leonardo leonardo

# Copia os arquivos do seu diretório app para o caminho /var/www/html dentro do container. Note que o caminho para qual você copia sua aplicação deve ser o diretório raíz
COPY ./app/ /var/www/html

# Copia as permissões do diretório atual do usuário leonardo no grupo leonardo, para os diretórios de /var/www/html/
COPY --chown=leonardo:leonardo . /var/www/html/

# Muda o usuário atual para leonardo
USER leonardo

# Expõe a porta 9000
EXPOSE 9000
#inicia o servidor do php-fpm
CMD ["php-fpm"]