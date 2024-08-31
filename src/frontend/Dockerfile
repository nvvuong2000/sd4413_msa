# Create image based on the official Node image from dockerhub
FROM node:lts-buster AS development

# Set environment variable
ENV DANGEROUSLY_DISABLE_HOST_CHECK=true

# Create app directory
WORKDIR /usr/src/app

# Copy dependency definitions
COPY src/frontend/package.json /usr/src/app/
COPY src/frontend/package-lock.json /usr/src/app/

# Install dependencies
RUN npm ci

# Copy the public and src directories explicitly
COPY src/frontend/public /usr/src/app/public
COPY src/frontend/src /usr/src/app/src

# Expose the port the app runs on
EXPOSE 3000

# Serve the app
CMD ["npm", "start"]
