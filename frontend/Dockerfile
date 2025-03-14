FROM node:18-alpine

WORKDIR /app

# Copy package.json and package-lock.json for caching
COPY package.json package-lock.json ./

# Install consistent dependencies
RUN npm install --frozen-lockfile

COPY . .

# default vite port
EXPOSE 5173

CMD ["npm", "run", "dev", "--", "--host", "0.0.0.0"]
