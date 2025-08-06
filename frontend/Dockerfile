# Use official Node.js runtime as base image
FROM node:22-alpine

# Set working directory in container
WORKDIR /app

# Copy package.json and package-lock.json (if available)
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of the application code
COPY . .

# Build the application
RUN npm run build

# Expose port 3000
EXPOSE 3000

# Use Vite preview to serve the built files
CMD ["npm", "run", "preview", "--", "--host", "0.0.0.0", "--port", "3000"]
