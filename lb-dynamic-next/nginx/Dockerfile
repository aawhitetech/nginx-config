FROM nginx:latest

# Copy static content to Nginx default directory
COPY conf.d /etc/nginx/conf.d

# Expose port 80
EXPOSE 80

# Start Nginx when the container starts
CMD ["nginx", "-g", "daemon off;"]