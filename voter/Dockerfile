# build environment
FROM node:18-alpine3.17 as build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# production environment
FROM nginx:stable-alpine
RUN apk update && apk add --upgrade curl 
COPY --from=build /app/build /usr/share/nginx/html/
COPY ./default.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD ["/bin/sh",  "-c",  "envsubst < /usr/share/nginx/html/env.template.js > /usr/share/nginx/html/env.js && exec nginx -g 'daemon off;'"]
