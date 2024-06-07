# Set Build Configuration.
ARG CONFIG_ENV=CONFIG_ENV


###################
# BUILD FOR PRODUCTION
###################
FROM node:20-alpine as builder
ARG CONFIG_ENV
WORKDIR /app
# installing npm packages
COPY package.json ./
RUN npm install
COPY . .
# build the app
RUN npm run build:$CONFIG_ENV
# clean all depencies
RUN rm -rf node_modules
RUN npm cache clean --force


###################
# PRODUCTION
###################
FROM node:20-alpine as production
WORKDIR /app
# Install Bash to execute shell scripts
RUN apk add --no-cache bash && \
    \
    # Install dependencies for serving the build
    npm install -g pm2 && \
    npm install -g serve && \
    \
    # Clean npm cache
    npm cache clean --force

#Copy build directory
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/package.json ./

# Expose the port that the application will run on
EXPOSE 3000

# Start the app using pm2-runtime with the specified script and name
CMD ["pm2-runtime", "start", "npm", "--", "start"]