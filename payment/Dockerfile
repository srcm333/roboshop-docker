# ---------- Builder Stage ----------
FROM python:3.9.25-alpine3.22 AS builder

WORKDIR /app

# Copy dependency file
COPY requirements.txt .

# Install build dependencies, install Python packages, then remove build dependencies
RUN apk add --no-cache --virtual .build-deps \
        gcc \
        musl-dev \
        linux-headers \
        python3-dev \
    && pip3 install --no-cache-dir --prefix=/install -r requirements.txt \
    && apk del .build-deps


# ---------- Runtime Stage ----------
FROM python:3.9.25-alpine3.22

WORKDIR /app

EXPOSE 8080

# Create non-root user
RUN addgroup -S roboshop && \
    adduser -S -G roboshop roboshop

# Copy installed Python packages from builder stage
COPY --from=builder /install /usr/local

# Copy application files
COPY --chown=roboshop:roboshop requirements.txt ./
COPY --chown=roboshop:roboshop payment.ini ./
COPY --chown=roboshop:roboshop *.py ./

# Ensure the application directory is owned by the application user
RUN chown -R roboshop:roboshop /app

# Run as non-root user
USER roboshop

# Start the application
CMD ["uwsgi", "--ini", "payment.ini"]


#################actual code from Sivakumar##################
# FROM nginx:1.24.0-alpine3.17-slim
# RUN rm -rf /usr/share/nginx/html/ && \
#     rm -rf /etc/nginx/nginx.conf && \
#     rm -rf /etc/nginx/conf.d/default.conf
# RUN mkdir -p /var/cache/nginx/client_temp && \
#     mkdir -p /var/cache/nginx/proxy_temp && \
#     mkdir -p /var/cache/nginx/fastcgi_temp && \
#     mkdir -p /var/cache/nginx/uwsgi_temp && \
#     mkdir -p /var/cache/nginx/scgi_temp && \
#     chown -R nginx:nginx /var/cache/nginx && \
#     chown -R nginx:nginx /etc/nginx/ && \
#     chmod -R 755 /etc/nginx/ && \
#     chown -R nginx:nginx /var/log/nginx

# RUN mkdir -p /etc/nginx/ssl/ && \
#     chown -R nginx:nginx /etc/nginx/ssl/ && \
#     chmod -R 755 /etc/nginx/ssl/

# RUN touch /var/run/nginx.pid && \
#     chown -R nginx:nginx /var/run/nginx.pid /run/nginx.pid
# COPY static/ /usr/share/nginx/html/
# COPY nginx.conf /etc/nginx/nginx.conf
# USER nginx
# CMD ["nginx", "-g", "daemon off;"]

################################################
# FROM nginx:1.24.0
# RUN rm -rf /usr/share/nginx/html/* 
# RUN rm -rf /etc/nginx/nginx.conf
# RUN rm -rf /etc/nginx/conf.d/default.conf
# COPY static/ /usr/share/nginx/html/
# COPY nginx.conf /etc/nginx/nginx.conf