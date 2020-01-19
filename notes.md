# Live Coding Steps

## Building the Dockerfile

1. All Docker files begin with a base image. This is marked by the 'FROM' key
  Let's pull in a base image and start using it:
  node:lts-alpine3.11
1. Looking for an image, check out hub.docker.com for a list of potential base images!
1. Next we need to run a few commands to build our image.
  Let's start with installing the latest npm command: RUN npm i npm@latest -g
1. We can set some environment variables:
  ENV NODE_ENV=production
1. Let's build a place to hold our code:
  RUN mkdir /opt/app && chown node:node /opt/app
1. and we can set this new space as the working directory for the rest of Dockerfile:
  WORKDIR /opt/app
1. as well as using the node user when we created the directory:
  USER node
1. Now we have an empty app directory, we need to copy code from our computer into that 'space':
  COPY ./package.json .
1. With a package.json we can install all of npm's dependencies:
  RUN npm i --silent \
      && npm cache clean --force
1. And have access to all the node_module executables:
  ENV PATH /opt/app/node_modules/.bin:$PATH
1. Ok almost done! We have our node modules but no runable code! Let's move that into the app directory:
  COPY ./dist .
1. Finally! Let's run our server:
  CMD ["node", "server.js"]

## Building The App

npx rollup --format=cjs --file=bundle.js -- index.mjs

## Building The Image

1. docker build -t tgql .

## Run The Image

1. docker run --name gql -p 3000:3000 tgql

## See the Running Contianer

1. ctop
