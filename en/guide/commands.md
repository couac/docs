---
title: Commands
description: Nuxt.js comes with a set of useful commands, both for development and production purpose.
---

> Nuxt.js comes with a set of useful commands, both for development and production purpose.

## List of Commands

| Command | Description |
|---------|-------------|
| nuxt | Launch a development server on [localhost:3000](http://localhost:3000) with hot-reloading. |
| nuxt build | Build your application with webpack and minify the JS & CSS (for production). |
| nuxt start | Start the server in production mode (`nuxt build` has to be ran before). |
| nuxt generate | Build the application and generate every route as a HTML file (used for static hosting). |

You should put these commands in the `package.json`:

```json
"scripts": {
  "dev": "nuxt",
  "build": "nuxt build",
  "start": "nuxt start",
  "generate" "nuxt generate"
}
```

Then, you can launch your commands via `npm run <command>` (example: `npm run dev`).

## Production Deployment

To deploy, instead of running nuxt, you probably want to build ahead of time. Therefore, building and starting are separate commands:

```bash
nuxt build
nuxt start
```

For example, to deploy with [now.sh](https://zeit.co/now) a `package.json` like follows is recommended:
```json
{
  "name": "my-app",
  "dependencies": {
    "nuxt": "latest"
  },
  "scripts": {
    "dev": "nuxt",
    "build": "nuxt build",
    "start": "nuxt start"
  }
}
```

Then run `now` and enjoy!

Note: we recommend putting `.nuxt` in `.npmignore` or `.gitignore`.

### Heroku Deployment

We recommend you to read the [Heroku documentation for node.js](https://devcenter.heroku.com/articles/nodejs-support).

First, we need to tell Heroku to install the `devDependencies` of the project (to be able to launch `npm run build`):
```bash
heroku config:set NPM_CONFIG_PRODUCTION=false
```

Also, we want our application to listen on the port `0.0.0.0` and run in production mode:
```bash
heroku config:set HOST=0.0.0.0
heroku config:set NODE_ENV=production
```

You should see this in your Heroku dashboard (Settings section):

![nuxt config vars Heroku](https://i.imgur.com/EEKl6aS.png)

Then, we tell Heroku to launch `npm run build` via the `heroku-postbuild` script in our `package.json`:
```js
"scripts": {
  "dev": "nuxt",
  "build": "nuxt build",
  "start": "nuxt start",
  "heroku-postbuild": "npm run build"
}
```

Finally, we can push the app on Heroku with:
```bash
git push heroku master
```

Voilà! Your nuxt.js application is now hosted on Heroku!

## Static Hosting Deployment

Nuxt.js gives you the possibility to host your web application on any static hosting like [surge.sh](https://surge.sh/) for example.

To deploy on surge.sh, first install it on your computer:
```bash
npm install -g surge
```

Then, we tell nuxt.js to generate our web application:

```bash
npm run generate
```

It will create a `dist` folder with everything inside ready to be deployed on a static hosting.

We can then deploy it to surge.sh:

```bash
surge dist/
```

Voilà :)

If you have a project with [dynamic routes](/guide/dynamic-routes), take a look at the [generate configuration](/api/configuration-generate) to tell nuxt.js how to generate these dynamic routes.

<div class="Alert">When generating your web application with `nuxt generate`, [the context](/api/pages-context) given to [data()](/guide/async-data#the-data-method) and [fetch()](/guide/vuex-store#the-fetch-method) will not have `req` and `res`.</div>
