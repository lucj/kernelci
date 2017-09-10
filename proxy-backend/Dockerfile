FROM nginx:1.13
COPY nginx.conf /etc/nginx/nginx.conf

# Create root directory
RUN mkdir -p /usr/share/nginx/html/kernelci \
    && chown www-data:www-data /usr/share/nginx/html/kernelci

# Backend configuration
COPY etc/backend-nginx.conf /etc/nginx/conf.d/kernelci.conf

# Upstream definitions
COPY etc/backend-upstreams.conf /etc/nginx/conf.d/backend-upstreams.conf

# Proxy / Proxy-cache
COPY etc/backend-proxy*.conf /etc/nginx/custom/
