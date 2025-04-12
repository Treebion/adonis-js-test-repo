# Stage 1: Build the application
FROM ubuntu:latest AS builder

# Install necessary packages and Node.js
RUN apt-get update && \
    apt-get install -y curl gnupg build-essential && \
    curl -fsSL https://deb.nodesource.com/setup_22.x | bash - && \
    apt-get update && \
    apt-get install -y nodejs

# Set working directory for building the project
WORKDIR /app

# Copy dependency manifests and install dependencies
COPY package*.json ./
RUN npm install

# Copy the rest of the application code
COPY . .

# Build the application (assumes you have a "build" script in package.json)
RUN npm run build

# Stage 2: Create the production image
FROM ubuntu:latest

# Install Node.js in the production image
RUN apt-get update && \
    apt-get install -y curl gnupg && \
    curl -fsSL https://deb.nodesource.com/setup_22.x | bash - && \
    apt-get update && \
    apt-get install -y nodejs

# Set production working directory
WORKDIR /app

# Copy only dependency manifests and install production dependencies
COPY package*.json ./
RUN npm install --production

# Copy the built application from the builder stage
COPY --from=builder /app/build ./build

# Expose the port your AdonisJS API listens on (default is 3333)
EXPOSE 3333

ENV TZ=UTC \
    PORT=3333 \
    HOST=0.0.0.0 \
    LOG_LEVEL=info \
    APP_KEY=jv5f1L1ocTVw7I0cPVZf0SVvIh1_NsQT \
    NODE_ENV=production \
    DB_HOST=127.0.0.1 \
    DB_PORT=5432 \
    DB_USER=root \
    DB_PASSWORD=root \
    DB_DATABASE=app \
    APP_NAME=adonis-api \
    LOG_LEVEL=info

# Set the command to run your AdonisJS API
CMD ["node", "build/bin/server.js"]