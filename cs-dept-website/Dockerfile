# ──────────────────────────────────────────────
# Stage 1: Build stage (optional linting step)
# ──────────────────────────────────────────────
FROM node:20-alpine AS builder

WORKDIR /app

# Install HTML/CSS linting tools
RUN npm install -g htmlhint stylelint stylelint-config-standard

# Copy source files
COPY src/ ./src/

# Create .htmlhintrc
COPY .htmlhintrc .htmlhintrc

# Run HTML linting (non-blocking warnings in build stage)
RUN htmlhint src/**/*.html || true

# ──────────────────────────────────────────────
# Stage 2: Production image using Nginx
# ──────────────────────────────────────────────
FROM nginx:1.25-alpine

# Remove default nginx config
RUN rm /etc/nginx/conf.d/default.conf

# Copy custom nginx config
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Copy static site files
COPY src/ /usr/share/nginx/html/

# Expose port 80
EXPOSE 80

# Health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD wget -qO- http://localhost/ || exit 1

CMD ["nginx", "-g", "daemon off;"]
