# What is Node.js?

Node is a JavaScript runtime that allows you to write JavaScript on the server!
It runs Chromes V8 JavaScript Engine, and is highly performant.

Node runs as a single, non-blocking process, without the use of multithreading:
its use of asynchronous I/O operations allows a single server to handle multiple
concurrent connections without blocking or the need to manage threats.

## JavaScript V8 Engine

Its the runtime that executes the JavaScript that has been delivered to your
browser (or Chrome, at least)! Many others exist for different browsers.

It also provides just-in-time compilation of JavaScript in order to speed up
execution.

## Running Node

We can run `node` from the command line like so:

    node app.js

## Exiting Node

We can programmatically exit a Node application using the `process.exit()`
method, which forces the process to terminate.

By default the exit code is 0, which means success. Different exit codes have
different meaning, which you might want to use in your own system to have the
program communicate to other programs.

This isn't very graceful. Typically, a program will exit gracefully once there
are no more operations to complete.

In the case of a server, we need to signal to the process to end. We can do this
by sending a SIGTERM signal, and then instructing the process how to handle that
signal with a callback:

    process.on('SIGTERM', () => {
      server.close(() => {
        console.log('Process terminated')
      })
    })

## Environment Variables

The `process` module includes an `env` property that is initalized when a
process starts. It holds all environment variables set at the start of the
process execution.

We can set environment variables directly from the command line:

    USER_ID=239482 USER_KEY=foobar node app.js

We can access these environment variables using `process.env.<VARIABLE_NAME>`

Typically, we will want to create a `.env` file in the root of our project, and
subsequently read them using the `dotenv` package during runtime.

## Passing Arguments from the Command Line

We can pass standalone or keyed arguements to our Node application from the
command line like so:

    node app.js joe
    node app.js name=joe

The `process` modeule exposes the `argv` property which contains all arguements
passed from the command line invokation. The first two arguements are the node
path and the executing file path, respectively. We can access these arguements
like so:

    // Iterate over all arugements
    process.argv.forEach((val, index) => {
      console.log(`${index}: ${val}`)
    })

    // Get additional arguements
    const args = process.argv.slice(2)
    args[0]

The `minimist` package is helpful for parsing keyed command line arguements:

    node app.js name=joe

    const args = require('minimist')(process.argv.slice(2))
    args['name'] //joe

## Outputing to the Command Line

Node provides the `console` module which provides an object that is pretty much
the same as the console in the browser!

The most basic and most used method is `console.log()`, which prints the string
you pass to it to the console. This prints to `stdout`.

`console.error` prints to the `stderr` stream.

We can use `console.trace()` to print the call stack trace of a function.

## Accepting Input from the Command Line

Node provides the `readline` module that allows us to collect input from a
stream such as `process.stdin`.

    const readline = require('readline').createInterface({
      input: process.stdin,
      output: process.stdout
    })

    readline.question(`What's your name?`, name => {
      console.log(`Hi ${name}!`)
      readline.close()
    })
