# Use the official lightweight Nginx image
FROM nginx:alpine

# Set working directory
WORKDIR /usr/share/nginx/html

# Copy your static site files (index.html, etc.)
COPY index.html /usr/share/nginx/html/

# If you have more assets (css/js/images), copy the whole folder:
# COPY ./html /usr/share/nginx/html

# Expose port 80 for HTTP traffic
EXPOSE 80

# Start Nginx in the foreground
CMD ["nginx", "-g", "daemon off;"]
