FROM node:18

# Set the working directory in the container
WORKDIR /usr/src/app

# Install build dependencies (for Node.js and Java)
RUN apt-get update && \
    apt-get install -y \
    build-essential \
    openjdk-17-jdk \
    && rm -rf /var/lib/apt/lists/*

# Copy package.json and package-lock.json (if available)
COPY package*.json ./

# Install node-gyp globally (if needed)
RUN npm install node-gyp

# Install dependencies
RUN npm install --no-cache

# Install node-pty with the latest tag
RUN npm install node-pty@latest

# Expose the port that your app runs on
EXPOSE 5000

# Copy the rest of your application code
COPY . .

# Rebuild native modules
RUN npm rebuild

# Start your Node.js or Java application
CMD ["node", "server.js"]
