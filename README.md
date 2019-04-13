# README

Webpack tutorial from =>
https://www.valentinog.com/blog/webpack-tutorial/


# Tutorial

Webpack is a tool for writing build scripts for javascript frontend
applications.

It uses a config file, typically `<name>.config.js`, to do this
build.

Notably, in webpack v4, there is no need to define a lot of things;
for instance, it has defaults for the entry point at `src/index.js`.

Let's first initial an npm project with `npm init -y` and then add an
`src/index.js` file. This will be our default to run.

```javascript
// src/index.js
console.log("Things");
```

Next, we'll add a default command to our `package.json` to ensure we
pull from `node_modules/` folder.

```javascript
// package.json
// ...
  "scripts": {
    "build": "webpack"
  }
// ...
```

Now, we'll run `npm run build` 

Webpack will use it's built-in output file to dump the build js to
`dist/main.js`. It will also complain about the lack of a default
mode...


## Modes in Webpack 4

Normally, you'd include two different config files: one for dev and
one for production. These both define slightly different configs
depending on where we're building.

In webpack 4, we can get away with this by simply calling `--mode
<mode>` where `<mode>` is either development or production.

If you call `--mode development`, you'll see that the resulting
bundled file _is not minified!_. This is faster than production mode. 

Production mode, by contrast, will do a series of optimizations out of
the box. This includes minification, scope hoisting, tree-shaking, and
so on.

input and outputs are defined with `webpack <entry.js> <output.js>`.


## Bable Transpiler

Modern JS uses ES6 -- which most browsers don't know how to handle.

In comes Babel, which can transpile ES6 so older browsers know how to
handle.

Webpack itself can't handle this transformation, so it has loaders to
do this. These are transformers.

Bable is inside of three core dependencies:

  * babel core  -- `@babel/core`
  * babel loader -- `babel-loader`
  * babel preset env -- `@babel/preset-env`

Next, you'll need to create a new file called `.babelrc` inside of the
project folder.

```javascript
// .babelrc
{
  "presets": [
    "@babel/preset-env"
  ]
}
```

Loading babel can be done in two ways:

  1. as a configuration file for webpack
  2. using `--module-bind` in your npm scripts

While we can get away with not having a webpack config file for most
things, at this point we _have_ to create one.

Therefore, we'll create a `webpack.config.js` file and configure the
loader in.

```javascript
module.exports = {
    module: {
        rules: [
            {
                test: /\.js$/,  // regex to define which files to load
                exclude: /node_modules/,   // don't include node_modules/ folder
                use: {  // which loaders to use
                    loader: "babel-loader"
                }
            }
        ]
    }
};
```

Note that we don't need to define our entry point as this still runs the defaults.

We'll add some ES6 into our javascript file and see what happens.

```shell
npm run build
```

Woo! It works.


## Using the `--module-bind` option

We can run this straight from the commandline. This is by calling
`--module-bind` and then passing options like `js=babel-loader`.

Run below to see what I mean:

```javascript
node_modules/webpack-cli/bin/cli.js \
  --mode development \
  --module-bind js=babel-loader
```

You can, of course, replace `node_modules/webpack-cli/bin/cli.js` with
`webpack` and stick it into your `package.json` file. Typically, it's
better to write separate webpack.config.js scripts though as its more
explicit with a more readable `package.json` file. 


