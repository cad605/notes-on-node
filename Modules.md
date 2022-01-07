# Expose functionality from a Node.js file using exports

We can import code exported by other packages / files using CommonJS modules:

    const library = require('./library')

The `module` package exposes the `exports` property, which can be assigned to an
object directly (thus exporting that object), or can have exported objects
assigned as properties:

    // car.js
    const car = {
      brand: 'Ford',
      model: 'Fiesta'
    }

    module.exports = car

    // index.js
    const car = require('./car')

Or

    // car.js
    exports.car = {
      brand: 'Ford',
      model: 'Fiesta'
    }

    // index.js
    const { car } = require('./items')

Thus, `module.exports` exposes the object it points to, while `exports` exposes
the properties of the object it points to.

## Package Managers

NPM is the standard package manager for Node. There are others, such as Yarn.

We can use a `package.json` file to list the dependencies in the project and
then simply run `npm install` to download them into our `node_modules` folder.

Or we can install a dependency directly using `npm install <PACKAGE_NAME>`,
which will add the dependency to our `package.json` file, and download to our
`node_modules`.

There are a number of important flags to this command as well:

1. --save-dev installs and adds the entry to the package.json file
   `devDependencies`
2. --no-save installs but does not add the entry to the package.json file
   `dependencies`
3. --save-optional installs and adds the entry to the package.json file
   `optionalDependencies`
4. --no-optional will prevent optional dependencies from being installed

The difference between `devDependencies` and `dependencies` is that the former
contains development tools, like a testing library, while the latter is bundled
with the app in production.

### Updating Packages and Versioning

We can update dependecies, checking all included packages for a newer version
using `npm update`

NPM also allows you to specify a packages version in your package.json file.

### Running tasks

We can specify running command line tasks in our package.json file. We can then
execute these tasks using `npm run <TASK_NAME>`

Here's an example:

    {
      "scripts": {
        "watch": "webpack --watch --progress --colors --config webpack.conf.js",
        "dev": "webpack --progress --colors --config webpack.conf.js",
        "prod": "NODE_ENV=production webpack -p --config webpack.conf.js",
      }
    }

    $ npm run watch
    $ npm run dev
    $ npm run prod

### But where are these packages installed?

With NPM, we can install packages such that they are available locally or
globally. Unless specified otherwise, when `npm install <PACKAGE_NAME>` is run,
NPM installs the dependency within a `node_modules` folder within the current
active directory, and updates the package.json that also resides there.

We can install globally using the `-g` flag, which will install the dependency
at the location specified by `npm root -g`

### Using a dependency once its installed

We need to import our dependency using `require` in order to use our dependency:

    const _ = require('lodash')

If our dependency is an executable, we can use `npx` to find the hidden `./bin`
folder within the packages `node_module` folder and execute the binaries. The
`./bin` folder contains symbolic links to the binaries!

### Understanding package.json

`package.json` is pretty much a manifest file for your project! It stores a list
of your dependencies and command line tasks, among other properties such:

1. `version` indicates the current version
1. `name` sets the application/package name
1. `description` is a brief description of the app/package
1. `main` sets the entry point for the application
1. `private` if set to true prevents the app/package to be accidentally
   published on npm
1. `scripts` defines a set of node scripts you can run
1. `dependencies` sets a list of npm packages installed as dependencies
1. `devDependencies` sets a list of npm packages installed as development
   dependencies
1. `engines` sets which versions of Node.js this package/app works on
1. `browserslist` is used to tell which browsers (and their versions) you want
   to support

### What is package-lock.json?

The `package-lock.json` sets your currently installed version of each package in
stone, and `npm` will use those exact versions when running `npm ci`.
