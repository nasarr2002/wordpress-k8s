FROM wordpress:php8.2-apache

# Installer utilitaires
RUN apt-get update && apt-get install -y \
    vim less zip unzip curl wget && \
    rm -rf /var/lib/apt/lists/*

# Copier un thème custom (tu peux modifier après)
COPY docker/theme/ /var/www/html/wp-content/themes/custom-theme/

# Permissions
RUN chown -R www-data:www-data /var/www/html

EXPOSE 80
