FROM nginx:1.13

# Create root directory
RUN mkdir -p /usr/share/nginx/html/kernelci \
    && chown nginx:nginx /usr/share/nginx/html/kernelci

# Remove default configuration
RUN rm /etc/nginx/conf.d/default.conf

# Backend configuration
COPY nginx.conf /etc/nginx/nginx.conf
COPY kernelci.conf /etc/nginx/conf.d/kernelci.conf

# Proxy path / proxy cache configuration
COPY proxy.conf /etc/nginx/custom/proxy.conf

# Maintenance
COPY maintenance.conf /etc/nginx/custom/maintenance.conf
