FROM php:5.6-apache

# --- CORREÇÃO DO DEBIAN STRETCH (MUSEU) ---
# Aponta para o arquivo morto e remove updates de segurança que não existem mais
RUN sed -i -e 's/deb.debian.org/archive.debian.org/g' \
           -e 's/security.debian.org/archive.debian.org/g' \
           -e '/stretch-updates/d' /etc/apt/sources.list

# --- INSTALAÇÃO COM PERMISSÃO PARA PACOTES ANTIGOS ---
# Adicionado flags para ignorar validade e autenticação (--allow-unauthenticated)
RUN apt-get update -o Acquire::Check-Valid-Until=false && apt-get install -y --allow-unauthenticated \
    libpq-dev \
    libpng-dev \
    gettext \
    locales \
    && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install pdo pdo_pgsql pgsql gd gettext mbstring

# Habilita o mod_rewrite do Apache
RUN a2enmod rewrite

# Configura Locale para pt_BR
RUN echo "pt_BR.UTF-8 UTF-8" > /etc/locale.gen && \
    locale-gen

WORKDIR /var/www/html