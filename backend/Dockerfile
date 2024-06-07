# Set Build Configuration.
ARG CONFIG_ENV=CONFIG_ENV


###################
# BUILD FOR PRODUCTION
###################
FROM node:20-alpine as builder

ARG CONFIG_ENV

WORKDIR /app
COPY package.json ./
RUN npm install -g npm@10.2.4
RUN npm install
COPY . .
# clean all depencies
RUN rm -rf node_modules
RUN npm cache clean --force
# install production dependencies only
RUN npm install --omit=dev


###################
# FINAL PRODUCTION IMAGE
###################
FROM node:20-alpine as runner
WORKDIR /app
# Install dependencies for serving the build
RUN npm install -g pm2 && \
    \
    # Clean npm cache
    npm cache clean --force
# Copy the built app from the builder image
COPY --from=builder . .
# Expose the port that the application will run on
EXPOSE 8888
# Start the application using PM2
CMD ["pm2-runtime", "start", "npm", "--", "start"]