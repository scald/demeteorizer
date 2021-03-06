Demeteorizer
=================

CLI tool to convert a Meteor app into a "standard" Node.js application. The resulting app contains
a package.json file with all required dependencies and can be easily ported to your own servers or
[Node.js PAAS providers](https://github.com/joyent/node/wiki/Node-Hosting).

Demeteorizer's output is similar to `meteor bundle` except that it generates a package.json containing
all required dependencies. This allows you to easily run `npm install` on the destination server,
which is especially important for compiled modules.

## Installing
    $ npm install -g demeteorizer

## Usage
    $ cd /path/to/meteor/app
    $ demeteorizer [options]

    -h, --help                    output usage information
    -V, --version                 output the version number
    -o, --output <path>           Output folder for converted application. Defaults to ./.demeteorized.
    -n, --node_version <version>  The required version of node [v0.10.25].
    -r, --release <version>       The Meteor version. Defaults to latest installed.
    -t, --tarball <path>          Output tarball path. If specified, creates a tar.gz of demeteorized application instead of directory.
    -a, --app_name <name>         Value to put in the package.json name field. Defaults to the current directory name.
    -p, --prerelease              Ignore Meteor prerelease warnings when running bundle.
    -d, --debug                   Bundle in debug mode (don't minify, etc).

## Examples
Convert the Meteor app in the current directory and output to ./.demeteorized

    $ demeteorizer

Convert the Meteor app in the current directory and output to ~/meteor-app/converted

    $ demeteorizer -o ~/meteor-app/converted

Convert the Meteor app in the current directory, output to ~/meteor-app/converted, and set minimum
node version to 0.8.26.

    $ demeteorizer -o ~/meteor-app/converted -n v0.8.26

## Running Resulting App
Meteor apps make use of the following environment variables:

    1. MONGO_URL='mongodb://user:password@host:port/databasename?autoReconnect=true'
    2. MAIL_URL='smtp://user:password@mailhost:port/' (optional)
    3. ROOT_URL='http://example.com' (optional)
    4. PORT=8080 (optional, defaults to 80)

Note that demeteorized applications still require a MongoDB connection in order
to correctly run. To run your demeteorized application locally, you will need
MongoDB [installed](http://docs.mongodb.org/manual/installation/) and running.

Run the app:

    $ cd /your/output/directory
    $ npm install
    $ MONGO_URL=mongodb://localhost:27017/test PORT=8080 node main

## Full Example
The following steps will create a Meteor example app, convert it, and run it.

    $ meteor create --example leaderboard
    $ cd leaderboard
    $ demeteorizer
    $ cd .demeteorized
    $ npm install
    $ MONGO_URL=[your-url] PORT=8080 node main.js

## Tarball
The --tarball option can be used to create a tar.gz of the application instead of putting the
converted app in a directory.

    $ demeteorizer --tarball /path/to/tarball.tar.gz

## Debug
The --debug option is passed to the meteor bundle command indicating to meteor
that the application should not be minified.

    $ demeteorizer --debug

## Support
Demeteorizer has been tested with the current Meteor example apps. If you find an app that doesn't
convert correctly, throw an issue in Github -
[https://github.com/onmodulus/demeteorizer/issues](https://github.com/onmodulus/demeteorizer/issues)

