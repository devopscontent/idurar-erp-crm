# ---------- 1) Build stage ----------
FROM node:20-alpine AS builder

# Create app directory
WORKDIR /app

# Copy package files first (for better Docker layer cache)
COPY package*.json ./

# Install dependencies (including dev, if any build step needs them)
RUN npm install

# Copy rest of backend source
COPY . .

# Optionally run tests or build step if backend has one
# RUN npm test
# For this project, backend is plain Node/Express, so no build step required.

# ---------- 2) Runtime stage ----------
FROM node:20-alpine

# Create non-root user
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

WORKDIR /app

# Copy only necessary files from builder
COPY --from=builder /app /app

# Install only production deps (if package-lock.json exists)
RUN npm install --omit=dev

# Expose backend port (we saw it's 8888)
EXPOSE 8888

# Use environment variable for DB URI (DATABASE)
ENV NODE_ENV=production

# Run as non-root
USER appuser

# Start server
CMD ["npm", "start"]

