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
RUN npm install -g pm2
COPY . .
# build the app
RUN npm run build:$CONFIG_ENV
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
# Copy the built app from the builder image
COPY --from=builder . .
# Expose the port that the application will run on
EXPOSE 8888
# Start the application using PM2
CMD ["npx", "pm2-runtime", "start", "npm", "run", "start"]