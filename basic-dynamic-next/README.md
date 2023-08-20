# Next.js Dynamic Content Sample

This sample demonstrates how to create a basic Next.js application that serves dynamic content and is further containerized within a Docker environment for deployment.

## Prerequisites

- Docker installed on your machine
- Node.js and npm (or Yarn) installed

## Steps

### 1. Initialize a New Next.js Application

You can use the `create-next-app` utility to quickly initialize a new Next.js project:

```bash
npx create-next-app next-dynamic-app
cd next-dynamic-app
```

### 2. Create a Dockerfile for the Next.js Application

In the root directory of your Next.js app, create a `Dockerfile`:

```Dockerfile
# Use an official Node.js runtime as the base image
FROM node:18

# Set the working directory in the container
WORKDIR /usr/src/app

# Copy package.json and package-lock.json (or yarn.lock) to the container
COPY package*.json ./

# Install Next.js and the project's dependencies inside the container
RUN npm install

# Copy the rest of the application to the container
COPY . .

# Build the Next.js app
RUN npm run build

# The application listens on port 3000, so expose it
EXPOSE 3000

# Define the command to run the app using npm
CMD ["npm", "start"]
```

### 3. Build the Next.js Docker Image

```bash
docker build -t next-dynamic-app .
```

### 4. Configure Nginx
```bash
cd {PROJECT_ROOT}/basic-dynamic-next/nginx
mkdir conf.d
```

### 5. Create a default.conf for Nginx
```default.conf
server {
    listen       80;
    listen  [::]:80;
    server_name  localhost;

    location / {
        proxy_pass http://next-dynamic-app:3000/;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header X-Forwarded-Host $host;
        proxy_set_header X-Forwarded-Port $server_port;
    }

    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }
}
```

### 6. Create a Dockerfile for Nginx
```Dockerfile
FROM nginx:latest

# Copy static content to Nginx default directory
COPY conf.d /etc/nginx/conf.d

# Expose port 80
EXPOSE 80

# Start Nginx when the container starts
CMD ["nginx", "-g", "daemon off;"]
```

### 7. Build the Nginx Docker Image

```bash
docker build -t my-nginx .
```

### 8. Run the Docker Containers

```bash
docker create network mynetwork
docker run -d --rm -p 8080:80 --network=mynetwork --name my-nginx my-nginx
docker run -d --rm -p 3000:3000 --network=mynetwork --name next-dynamic-app next-dynamic-app
```

### 6. Access the Dynamic Page

Open a browser and navigate to `http://localhost:8080`. You'll see the next.js app displayed.

## Conclusion

You've successfully created a Next.js app serving dynamic content and containerized it with Docker for deployment.
