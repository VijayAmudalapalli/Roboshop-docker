# FROM python:3.9.25-alpine3.21 AS builder
# # Only requirements first for caching
# COPY requirements.txt .
# # Install into /install (just like you did)
# RUN pip3 install --prefix=/install -r requirements.txt


# # ===========================
# # Stage 2: Runtime (Alpine)
# # ===========================
# FROM python:3.9.25-alpine3.21
# EXPOSE 8080
# WORKDIR /opt/server
# # Runtime deps only
# RUN apk add --no-cache pcre
# # Create non-root user
# RUN addgroup -S roboshop && adduser -S roboshop -G roboshop
# # Copy installed packages from builder
# COPY --from=builder /install /usr/local
# # Copy app files
# COPY --chown=roboshop:roboshop payment.ini *.py requirements.txt .
# ENV CART_HOST="cart" \
#     CART_PORT="8080" \
#     USER_HOST="user" \
#     USER_PORT="8080" \
#     AMQP_HOST="rabbitmq" \
#     AMQP_USER="roboshop" \
#     AMQP_PASS="roboshop123"

# USER roboshop
# CMD ["uwsgi", "--ini", "payment.ini"]


# FROM python:3.9
# EXPOSE 8080
# WORKDIR /opt/server
# COPY requirements.txt .
# COPY *.py .
# COPY payment.ini .
# RUN pip3 install -r requirements.txt
# ENV CART_HOST="cart" \
#     CART_PORT="8080" \
#     USER_HOST="user" \
#     USER_PORT="8080" \
#     AMQP_HOST="rabbitmq" \
#     AMQP_USER="roboshop" \
#     AMQP_PASS="roboshop123"
# CMD ["uwsgi", "--ini", "payment.ini"]


# Gemini AI Code:
# ===========================
# Stage 1: Builder
# ===========================
# 1. We MUST start with a FROM instruction. 
# We name it 'builder' so Stage 2 can refer to it later.
FROM python:3.9.25-alpine3.21 AS builder

WORKDIR /opt/server

# 2. Install compilation tools (Needed for uWSGI/Python on Alpine)
# Without this, pip install will likely fail for packages needing C extensions.
RUN apk add --no-cache build-base linux-headers python3-dev

# 3. Now run your original caching logic
COPY requirements.txt .
RUN pip3 install --prefix=/install -r requirements.txt


# ===========================
# Stage 2: Runtime (Alpine)
# ===========================
FROM python:3.9.25-alpine3.21

WORKDIR /opt/server

# Runtime deps only (pcre is needed for uWSGI runtime)
RUN apk add --no-cache pcre

# Create non-root user
RUN addgroup -S roboshop && adduser -S roboshop -G roboshop

# Copy installed packages from the 'builder' stage
# Python --prefix installation usually maps nicely to /usr/local
COPY --from=builder /install /usr/local

# Copy app files
COPY --chown=roboshop:roboshop payment.ini *.py requirements.txt .

ENV CART_HOST="cart" \
    CART_PORT="8080" \
    USER_HOST="user" \
    USER_PORT="8080" \
    AMQP_HOST="rabbitmq" \
    AMQP_USER="roboshop" \
    AMQP_PASS="roboshop123"

USER roboshop

# Expose port (Documentation only, strictly speaking)
EXPOSE 8080

CMD ["uwsgi", "--ini", "payment.ini"]


# # ChatGPT code :
# # ===========================# ===========================
# # Stage 1: Builder
# # ===========================
# FROM python:3.9.25-alpine3.21 AS builder

# # Only requirements first for caching
# COPY requirements.txt .

# # Install into /install (local folder)
# RUN pip3 install --prefix=/install -r requirements.txt



# # ===========================
# # Stage 2: Runtime
# # ===========================
# FROM python:3.9.25-alpine3.21

# EXPOSE 8080
# WORKDIR /opt/server

# # Runtime deps only
# RUN apk add --no-cache pcre

# # Create non-root user
# RUN addgroup -S roboshop && adduser -S roboshop -G roboshop

# # Copy installed packages from builder
# COPY --from=builder /install /usr/local

# # Copy app files
# COPY --chown=roboshop:roboshop payment.ini *.py requirements.txt .

# ENV CART_HOST="cart" \
#     CART_PORT="8080" \
#     USER_HOST="user" \
#     USER_PORT="8080" \
#     AMQP_HOST="rabbitmq" \
#     AMQP_USER="roboshop" \
#     AMQP_PASS="roboshop123"

# USER roboshop

# CMD ["uwsgi", "--ini", "payment.ini"]
