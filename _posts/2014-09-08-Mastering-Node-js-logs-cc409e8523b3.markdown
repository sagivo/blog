---
layout: post
title: Mastering Node.js logs
description: ''
date: '2014-09-08T18:27:19.000Z'
categories: []
keywords: []
slug: /@sagivo/mastering-node-js-logs-cc409e8523b3
---

It’s very easy to debug your node.js application when you’re in development mode, most of the times you just watch on the console output. This is usually good enough to spot some errors and print some states.

But what happens when you decide to go live? How to access the logs? How to create different log levels?

```javascript
console.log('Free Willy!');
```

#### Manual Logging

If you want to log only specific things that you want to track, there are easy libraries that let you do so. [Winston](https://github.com/flatiron/winston) is a very popular one. You can log your output to multiple transports at the same time (e.g: file, console…). It’s also supported by 3rd parties cloud services that will be happy to host those outputs for you (more about that next).

Here is a simple snippet of how to configure winston and log it to both your console and a file:

```javascript
var winston = require('winston');
//log your output to a file also
winston.add(winston.transports.File, { filename: 'somefile.log' });
//log some outputs
winston.log('info', 'Hello distributed log files!');
winston.error('Who let the dogs out?!');
```

Other common logging tools are [node-bunyan](https://github.com/trentm/node-bunyan), [log4js](https://github.com/nomiddlename/log4js-node) and [loggly](https://github.com/nodejitsu/node-loggly).

#### Hosting Logs

You don’t have to login your server and open a file (or even worse — looking at the console) each time you want to see your latest logs.

There are platy of cloud services that will host your logs so you can access them remotely, aggregate them, query them and even define some alerts for some patterns.

[Logentries](https://logentries.com/) has a free plan and can easily configure to work with node. To configure, you’ll have to use the [node-logentries](https://github.com/rjrodger/node-logentries) library.

```javascript
var logentries = require('node-logentries');
var log = logentries.logger({token:'YOUR_TOKEN'});
log.info("I think pandas are ugly");
//or connect via winston
var winston = require('winston');
log.winston(winston);
```

Another logging services is [Loggly](https://www.loggly.com/) — very similar concept and also has a [node sdk](https://github.com/nodejitsu/node-loggly) to directly write logs to this service.

#### Where Is My Console Output?

So far we talked about logging some data that you choose, but there are lots of times you just want to watch all the output your console has and even track errors.

In order to stream your console output we have to understand first how it works.

#### stdout & stderr

Standard output (stdout) is the stream where a program writes its output data. The program requests data transfer with the write operation. Unless redirected, standard output is the text terminal which initiated the program.

Standard error (stderr) is another output stream typically used by programs to output error messages or diagnostics. It is a stream independent of standard output and can be redirected separately. The usual destination is the text terminal which started the program to provide the best chance of being seen even if standard output is redirected (so not readily observed). For example, output of a program in a pipeline is redirected to input of the next program, but errors from each program still go directly to the text terminal.

When your node app is running, it uses stdout and stderr as it’s output. Each time the software needs to print something (via console.log() or any other commend) it prints it to stdout.

Each time an error occurred, the system prints it to stderr.

In production you can’t watch your output (end should not since your node app should run as a daemon process, usually via upstart or forever), this is why it’s better to re-configure stdout and stderr.

There are several ways to configure it, you can do it within the app by creating another [child-process](http://nodejs.org/api/child_process.html) that handles incoming data. Child-process use spawn which is an EventEmitter object so it’s non-blocking and recommended.

```js
var child = require('child_process');
var myREPL = child.spawn('bash');
//pipe node process to new child process
process.stdin.pipe(myREPL.stdin);
myREPL.stdin.on("end", function() { process.exit(0);});
//hook to incomming data event
myREPL.stdout.on('data', function (data) {
  console.log('we got new data here!',data);
});
myREPL.stderr.on('data', function (data) {
  console.log('oh oh :(' + data);
});
```

An easier way to configure the stderr and stdout is by starting the node app. Simply use:

```js
#log both output and error to same file
node app.js >> /path/to/app.log 2>&1
#use seperate files for errors and outputs
node app.js >> /path/to/app.log 2>> /path/to/err.log
```

This will tell the node process to redirect all the output to the files you specify. You can also change the \`>>\` sign to \`>\` if you want to override the previous file each time you start the app.

Once you pipe all your outputs to files you can use the services i mentioned above to track those files in real time and display them there. Here is more info about how to track file using [Logentries agent](https://logentries.com/doc/agent/)

and using [Loggly](https://community.loggly.com/customer/portal/articles/1225986-rsyslog-configuration?b_id=50#).

#### Even More Configurable Logging

But wait, that’s not all! Now that we know how to pipe our outputs to files/services we can even control the syntax we want in some cases.

We can connect to specific events and control the output.

Here are some samples for handling with express.js and mongoose:

```js
var express = require('express');
var app = express();
//configure express logging
var logStream = {
  write: function(message, encoding) {
    return console.log("EXPRESS: " + message);
  }
};
app.use(express.logger({stream: logStream}));
//configure mongoose logging 
var mongoose = require('mongoose');
mongoose.set('debug', function(collectionName, method, query, doc){
  console.log("DB: ", collectionName, method, JSON.stringify(query));
});
```

#### The Bottom line

Logs are important tool both for development and production. There are manny ways to define where the log output should go and how to see it, use the one suites your needs and follow your logs :)