# ---------- Stage 1 : Build the React application ----------
FROM node:14 AS builder

# Create and switch to the working directory
WORKDIR /app

# Copy dependency manifest first
COPY package*.json ./

# Install project dependencies
RUN npm install

# Copy the rest of the application source code
COPY . .

# Build the optimized production files
# Backend URLs supplied during Docker build
ARG REACT_APP_COMMON_DATA_SERVICE_URL
ARG REACT_APP_AUTHENTICATION_SERVICE_URL
ARG REACT_APP_SEARCH_SUGGESTION_SERVICE_URL
ARG REACT_APP_PAYMENT_SERVICE_URL

ENV REACT_APP_COMMON_DATA_SERVICE_URL=$REACT_APP_COMMON_DATA_SERVICE_URL
ENV REACT_APP_AUTHENTICATION_SERVICE_URL=$REACT_APP_AUTHENTICATION_SERVICE_URL
ENV REACT_APP_SEARCH_SUGGESTION_SERVICE_URL=$REACT_APP_SEARCH_SUGGESTION_SERVICE_URL
ENV REACT_APP_PAYMENT_SERVICE_URL=$REACT_APP_PAYMENT_SERVICE_URL
RUN npm run build

# ---------- Stage 2 : Production Image ----------
FROM nginx:alpine

# Copy the built static files from the builder stage
COPY --from=builder /app/build /usr/share/nginx/html

# Expose the HTTP port
EXPOSE 80

# Start Nginx in the foreground
CMD ["nginx", "-g", "daemon off;"]
