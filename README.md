**************************
React GraphQL Playground
**************************

Description
===========

React and GraphQL development environment in docker.

How?
----

1. Start mysql and prisma:
```
docker-compose up -d prisma
```

2. Initialize a new project:
```
docker run --rm \
    --volume ${PWD}:/home/node/src \
    --workdir /home/node/src \
    --user node \
    --entrypoint npm \
node:11-alpine init -y
```

3. Create a react app:
```
docker run --rm \
    --user node \
    --volume ${PWD}:/home/node/src \
    --workdir /home/node/src \
    --entrypoint npx \
node:11-alpine create-react-app my-app
```

4. Start the react server:
```
docker run --rm \
    --detach \
    --user node \
    --volume ${PWD}:/home/node/src \
    --workdir /home/node/src/my-app \
    --network prisma_prisma \
    --publish 3000:3000 \
    --name my-app \
    --entrypoint yarn \
node:11-alpine start
```

5. Install project dependencies (include optional dependencies i.e.: `@material-ui/core`, `@material-ui/icons`, `@material-ui/lab`, `typescript`):
```
docker exec \
    --user node \
my-app npm install graphql-cli prisma prisma-client-lib
```

6. Initialize the prisma project:
```
docker exec \
    --user node \
    --env PATH=/usr/local/bin:/home/node/src/my-app/node_modules/.bin \
my-app prisma init --endpoint http://prisma:4466
```

7. Deploy the prisma datamodel:
```
docker exec -it \
    --user node \
    --env PATH=/usr/local/bin:/home/node/src/my-app/node_modules/.bin \
my-app prisma deploy
```

8. [Generate your prisma client](https://www.prisma.io/docs/1.27/get-started/01-setting-up-prisma-new-database-JAVASCRIPT-a002/#generate-your-prisma-client "Generate your Prisma client"):
```
docker exec \
    --user node \
    --env PATH=/usr/local/bin:/home/node/src/my-app/node_modules/.bin \
my-app prisma generate
```
