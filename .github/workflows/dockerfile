FROM node:14

# Create app directory
WORKDIR /app

# Install app dependencies
COPY package*.json ./
RUN npm install

# Bundle app source
COPY . .

# Build the Backstage app
RUN npm run build

# Expose the port the app runs on
EXPOSE 7000

# Start the app
CMD ["npm", "start"]
