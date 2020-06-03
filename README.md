# Front To Back

```bash
mkdir newproject
cd newproject
npx create-react-app client --use-npm
npx express-generator --view=ejs --git server
sudo rm -r /client.git
git init
git add .
git add remote
git push
```

## If you have the Docker Plugin for VSCode, F1 will automate most of the dockerfiles for you..

before deleting the **_`Dockerfile`_** from the root of the project, copy it to each of the **_`client`_** and **_`server`_** directories. Then Edit the **_`docker-copmpose.yml`_** to launch both containers in the **_`default network`_** at the same time on different **_`ports`_**.

## **_`client/Dockerfile`_**

```Dockerfile
FROM node:10.13-alpine
ENV NODE_ENV production
WORKDIR /usr/src/app
COPY ["package.json", "package-lock.json*", "npm-shrinkwrap.json*", "./"]
RUN npm install --production  && mv node_modules ../
COPY . .
EXPOSE 3000
CMD npm start
```

## **_`server/Dockerfile`_**

```Dockerfile
FROM node:10.13-alpine
ENV NODE_ENV production
WORKDIR /usr/src/app
COPY ["package.json", "package-lock.json*", "npm-shrinkwrap.json*", "./"]
RUN npm install --production && mv node_modules ../
COPY . .
EXPOSE 3000
CMD npm start
```

## **_`docker-copmpose.yml`_**

```yml
version: '2.4'

services:
  client:
    image: client
    build: ./client
    environment:
      NODE_ENV: production
    ports:
      - 3000:3000
    stdin_open: true
    tty: true

  server:
    image: server
    build: ./server
    environment:
      NODE_ENV: production
    ports:
      - 3001:3000
```
