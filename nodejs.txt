FROM node:18

# Set the working directory in the container
WORKDIR /usr/src/app

# Install build dependencies
RUN apt-get update && \
    apt-get install -y \
    build-essential \
    && rm -rf /var/lib/apt/lists/*

# Copy package.json and package-lock.json (if available)
COPY package*.json ./

# Install node-gyp globally (if needed)
RUN npm install node-gyp

# Install dependencies
RUN npm install --no-cache

# Install node-pty with the latest tag
RUN npm install node-pty@latest

# Copy the entrypoint script into the image
# COPY entrypoint.sh /usr/src/app/entrypoint.sh

# Make the entrypoint script executable
# RUN chmod +x /usr/src/app/entrypoint.sh

# Set the entrypoint to your script
# ENTRYPOINT ["/usr/src/app/entrypoint.sh"]

# Expose the port that your app runs on
EXPOSE 5000
EXPOSE 3000

# Copy the rest of your application code
COPY . .

# Rebuild native modules
RUN npm rebuild

# Start your application
CMD ["node", "server.js"]
