# syntax=docker/dockerfile:experimental
FROM node:10 as build
WORKDIR /usr/src/app
COPY package.json ./
COPY package-lock.json ./
RUN npm install
COPY . ./
RUN npm run build

FROM nginx:stable-alpine
COPY nginx.vh.default.conf /etc/nginx/conf.d/default.conf
COPY --from=build /usr/src/app/build /usr/share/nginx/html
WORKDIR /usr/share/nginx/html

RUN sed -i.bak 's/^user/#user/' /etc/nginx/nginx.conf
RUN sed -i.bak 's/listen\(.*\)80;/listen 8080;/' /etc/nginx/conf.d/default.conf
EXPOSE 8080

RUN apk add --no-cache nodejs npm dos2unix
RUN npm install -g @beam-australia/react-env
ADD entrypoint.sh /var/entrypoint.sh
RUN chmod -R g+rwx /var/cache/nginx /var/run /var/log/nginx /usr/share/nginx /var/entrypoint.sh
RUN dos2unix /var/entrypoint.sh
ENTRYPOINT ["/var/entrypoint.sh"]

CMD ["nginx", "-g", "daemon off;"]