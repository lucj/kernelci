FROM nginx:1.13

# Create root directory
RUN mkdir -p /usr/share/nginx/html/kernelci \
    && chown www-data:www-data /usr/share/nginx/html/kernelci

# Backend configuration
COPY nginx.conf /etc/nginx/conf.d/kernelci.conf

# Upstream definitions
COPY upstreams.conf /etc/nginx/conf.d/upstreams.conf

# Proxy / Proxy-cache
COPY proxy*.conf /etc/nginx/custom/
