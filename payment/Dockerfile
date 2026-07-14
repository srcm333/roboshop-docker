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