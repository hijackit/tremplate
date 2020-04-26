# Tremplate
Tremplate is a small template project for a React/Typescript application with Babel and Webpack.

Every library used by this template has been included for a good reason, and the following sections explains these reasons.  

The rationale behind this template is to include only essential libraries needed to build a react application in typescript. This template aims to be a good starting point that can be easily extended with whatever library a user may needed.

## Usage
Just clone the repo and modify the sources or just use the following files as a reference to build your own project:
* `package.json`: contains all the runtime and development dependencies described in this readme
* `tsconfig.json`: the typescript configuration file
* `babel.config.json`: the babel configuration file
* `webpack.config.js`: the webpack configuration file

## Typescript
One of the main goal of this template is to allow us to write Typescript code. For this we can simply add the `typescript` library, but typescript is much more useful when we include the type definition of the libraries that we will use in the project, that's why we include the type definitions for react:
* `@types/react`
* `@types/react-dom`

With the typescript library now we can transpile typescript code into javascript code with the typescript compiler `tsc`.  
It is important to note that from a given typescript code it is possible to obtain slightly different "flavors" of javascript code: the exact behavior of the compilation process can be configured through the `tsconfig.json` configuration file, where we can configure things like:
* where to look for the source files
* the output directory
* how to handle JSX blocks
* the type of javascript module system to use
* and more...

At this point we can write typescript code and compile it into javascript code. For every typescript file we will obtain its javascript counterpart file.

Now, even though we *could* manually compile all the typescript sources into javascript files with the `tsc` command, there are other tools that makes this process easier *while* solving other problems that we need to tackle anyway. That's why we want to use babel and webpack.

## Babel
Babel is a tool that converts the code written by the developer into something understandable by the browser.  
We include it as a dev-dependency:
* `@babel/core`: core library of babel

By default babel does nothing: if you process a file, it will return the same file untouched. That's why we need to add babel plugins.  
But which plugin? It depends on mainly two things: 
* the code that the developer writes (e.g. languange and version) 
* the target browsers that we want to support (e.g. vendors and versions)

For our use-case, we need babel to:
* convert the latest javascript syntax features into equivalent code supported by different versions of different browsers
* convert JSX in javascript
* convert typescript in javascript

Since these requirements are quite common, the babel team came up with a common set of plugins. These "set of plugins" are called preset, and we will use the following ones:
* `@babel/preset-env`: to create code understandable by the majority of browsers used out there
* `@babel/preset-react`: to support JSX
* `@babel/preset-typescript`: to support Typescript code

In our dev dependencies we will also use the following babel packages:

* `@babel/cli`: allows you to use babel from the terminal
* `@babel/plugin-proposal-class-properties`: needed to use the not-already-standard Javascript class properties (or class fields)

Installing babel plugins does not tell babel to use those plugins: in order to do so, we need a babel configuration file `babel.config.js` in wich we simply declare that we want to use the presets and the plugin.

At this point we can write code in typescript, use JSX syntax and leverage all the features of "modern" javascript/typescript and let Babel take care of compiling all these stuff into "plain and simple" javascript.

## Webpack
Once we have a bunch of "plain and simple" javascript files, we still need something that:
* creates a single js file (bundle) from our sources *and* from our depencencies
* automatically "inject" the bundle into our index.html

And other not-essential yet-useful features:
* serve the site locally during development with hot-reloading
* includes source-maps to the bundle
* (pre) process CSS files
* rename assets (like images) in order to avoid stale-data when assets gets updated
* and more...

For our template project we need to include the core webpack packages:
* `webpack`
* `webpack-cli`

__webpack and babel__: since we don't want to manually call babel *and* webpack, we use the `babel-loader` package. In this way we only need to call webpack, that in turn will call babel for us.

__dev-server__: another essential feature that we want is the development server with hot-reloading features with the `webpack-dev-server` package

__cleanup__: before each new build, we want to cleanup the build folder; for this we need the `clean-webpack-plugin`

__bundle injection__: we need to include the javascript in our index.html page. We can do this manually or leave it to webpack, using `html-webpack-plugin`

__css management__: we can write the css file as a static resource and manually include it in our index.html file *or* we can treat the css file(s) as sources  which can be referenced (imported) from our typescript files and managed by webpack that will also inject the css into our index.html for us. For this we will use `css-loader` and `mini-css-extract-plugin`. The first one allows us to import css in typescript files, the second one generates the CSS files and injects them into our index.html page.

__css assets__: [TODO] what if our css files references some images?

## References
* [Microsoft TypeScript-Babel-Starter](https://github.com/microsoft/TypeScript-Babel-Starter)
* [freeCodeCamp webpack 4 tutorial](https://www.freecodecamp.org/news/creating-a-production-ready-webpack-4-config-from-scratch/)
