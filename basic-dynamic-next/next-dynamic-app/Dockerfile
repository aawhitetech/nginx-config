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