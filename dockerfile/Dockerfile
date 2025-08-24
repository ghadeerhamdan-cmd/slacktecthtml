# Use a lightweight Nginx image as the base
    FROM nginx:alpine

    # Copy index.html into the Nginx web root directory
    COPY index.html /usr/share/nginx/html/index.html

    # Expose port 80 for web traffic
    EXPOSE 80

    # Command to run Nginx when the container starts
    CMD ["nginx", "-g", "daemon off;"]