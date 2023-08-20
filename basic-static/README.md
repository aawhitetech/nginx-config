# Nginx Docker Container for Static Content

This guide will walk you through the process of setting up a basic Nginx web server within a Docker container to serve static content.

## Prerequisites

- Docker installed on your machine
- Basic understanding of Docker and Nginx

## Steps

### 1. Create Your Static Content

Create a directory to house your static content, e.g., `website-content`. Inside this directory, you can put your HTML, CSS, JS files, and any other static content you wish to serve.

```bash
mkdir website-content
echo "<h1>Hello, World!</h1>" > website-content/index.html
```

### 2. Create a Dockerfile

Create a `Dockerfile` in the root directory:

```Dockerfile
FROM nginx:latest

# Copy static content to Nginx default directory
COPY website-content /usr/share/nginx/html

# Expose port 80
EXPOSE 80

# Start Nginx when the container starts
CMD ["nginx", "-g", "daemon off;"]
```

### 3. Build the Docker Image

Navigate to the directory containing your `Dockerfile` and run the following command to build the Docker image:

```bash
docker build -t my-nginx-webserver .
```

This will create a Docker image named `my-nginx-webserver`.

### 4. Run the Docker Container

Run a container instance of your image:

```bash
docker run --name nginx-container -d -p 8080:80 my-nginx-webserver
```

This will start a new container named `nginx-container` and bind its port 80 to port 8080 on the host machine.

### 5. Access the Website

Open a web browser and navigate to `http://localhost:8080`. You should see the "Hello, World!" message.

## Conclusion

You've successfully set up a basic Nginx web server to serve static content from within a Docker container. Modify the `website-content` directory as needed to update or expand your website.

---

This README is a basic starting point, and there's a lot more you can do with Docker and Nginx. Depending on your project requirements, you may want to explore more advanced configurations, including setting up SSL, using Docker Compose for multi-container setups, and integrating with other services.