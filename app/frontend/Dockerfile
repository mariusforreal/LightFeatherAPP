FROM node:16-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
# Update the API URL environment variable during build
ARG REACT_APP_API_URL
ENV REACT_APP_API_URL=${REACT_APP_API_URL}
RUN npm run build

# Use a lightweight server to serve the static files
FROM nginx:alpine
COPY --from=build /app/build /usr/share/nginx/html
# Either copy from first stage if nginx.conf was part of your app code
COPY --from=build /app/nginx.conf /etc/nginx/conf.d/default.conf
# OR copy directly from build context if it's alongside Dockerfile
# COPY ./nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 3000
CMD ["nginx", "-g", "daemon off;"]
