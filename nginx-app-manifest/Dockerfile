# Use a specific, stable Nginx Alpine version
FROM nginx:1.27.2-alpine

# Update Alpine packages to patch vulnerabilities
RUN apk --no-cache upgrade

# Set working directory
WORKDIR /usr/share/nginx/html

# Copy static site files
COPY index.html /usr/share/nginx/html/

# Expose HTTP port
EXPOSE 80

# Run Nginx in foreground
CMD ["nginx", "-g", "daemon off;"]

