[![Deploy to now](https://deploy.now.sh/static/button.svg)](https://deploy.now.sh/?repo=https://github.com/zeit/next.js/tree/master/examples/custom-server-express)

# Custom Express Server example

## How to use

### Using `create-next-app`

Execute [`create-next-app`](https://github.com/segmentio/create-next-app) with [Yarn](https://yarnpkg.com/lang/en/docs/cli/create/) or [npx](https://github.com/zkat/npx#readme) to bootstrap the example:

```bash
npx create-next-app --example custom-server-express custom-server-express-app
# or
yarn create next-app --example custom-server-express custom-server-express-app
```

### Download manually

Download the example:

```bash
curl https://codeload.github.com/zeit/next.js/tar.gz/canary | tar -xz --strip=2 next.js-canary/examples/custom-server-express
cd custom-server-express
```

Install it and run:

```bash
npm install
npm run build
# remove node_modules
npm i --production
npm start

# or
yarn
yarn run build
# remove node_modules
yarn install --production
yarn run start
```

Deploy it to the cloud with [now](https://zeit.co/now) ([download](https://zeit.co/download))

```bash
now
```

## The idea behind the example

Most of the times ~~the default Next server will be enough but sometimes~~ you want to run your own server to customize routes or other kind of the app behavior. Next provides a [Custom server and routing](https://github.com/zeit/next.js#custom-server-and-routing) so you can customize as much as you want.

By requiring `next-server` instead of `next` in server.js you can eliminate production dependensy to webpack and other development modules.
To do so you first need to bundle next application using `next build` with devDependencies installed, then leave only production `dependencies` required to run the server.

Note: Bundles produced by `next build` have references to `core-js` (v2.x.x), `regenerator-runtime` and `next/router`. First 2 you can add manually to dependencies. `next/router` is part of next, so you have to transpile it using `next-transpile-modules` plugin to avoid runtime reference to `next` module. To do so you need following in `next.config.js`:
```
const withTM = require('next-transpile-modules');
 
module.exports = withTM({
  transpileModules: ['next']
});
```

Because the Next.js server is just a node.js module you can combine it with any other part of the node.js ecosystem. in this case we are using express to build a custom router on top of Next.

The example shows a server that serves the component living in `pages/a.js` when the route `/b` is requested and `pages/b.js` when the route `/a` is accessed. This is obviously a non-standard routing strategy. You can see how this custom routing is being made inside `server.js`.
