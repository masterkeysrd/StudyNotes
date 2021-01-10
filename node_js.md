# NodeJS Notes

Node.js is single-threaded JS Engine base in Google V8 Engine. In Node all tasks share the same thread.

## NodeJS Globals

### Global Object
To make a simple hello world program in NodeJS just create a file with code below.

```javascript
let hello = 'Hello from NODE JS'
console.log(hello);
```

When you create the file run the command `node filename.js`

The console object belongs to global object, it means if we change `console.log` by `global.console.log` we going to have the same result.

```javascript
let hello = 'Hello from NODE JS'
global.console.log(hello);
```

Other objects that are present in the global object are:

* `__dirname`: This object get the current dirname as  a string.
* `__filename`: This object get the current filename as a string.

### Argument variables with process.argv

The object process belongs to the global object and it have a lot info about the process that is running node.js. For example we can get info about the process, about, arguments passed, environment variables and other stuffs with this object.

```javascript
// This code use the process object 'global.process'

// Get the PID (process ID) of the node program that is executing the file.
console.log(process.pid);

// Get the version of node.js that is executing the file
console.log(process.versions.node);
```

We can access to the arguments with the code below.

```javascript
// This code will show an array of all the arguments passed to Node executing the file.
console.log(process.argv);
```

### Standard Output

The standard output in node.js is represent by the object `process.stdout` and implements the `write` method.

```javascript
// This example write a message in the standard output
process.stdout.write('Hello ');
process.stdout.write('World \n\n\n');

process.stdout.write('Is cool learn node.js \n');
```

### Standard Input

The standard output in node.js is represent by the object `process.stdin` and implement the method `on`  that is a wait for a event.

```javascript
// This example wait data from the standard out and display it on standard output.
process.stdin.on('data', data => {
    process.stdout.write(`\n\n ${data.toString().trim()}`);
    process.exit();
});
```

### Timing Functions

The timing functions are other way to work async on node.js and these functions work in node equals than the browser.

#### setTimeout

This function creates a delay.

```javascript
// This example will create a wait 3 seconds to execute timerFinished function.
const waitTime = 3000;
console.log(`setting a ${waitTime / 1000} seconde delay`);

const timerFinished = () => console.log("done");

setTimeout(timerFinished, waitTime);
```

#### setInterval

This function executes a function each time that the interval time passes.

```javascript
// This example creates a interval and the are passed 5 seconds stops the interval
const waitInterval = 500;

let currentTime = 0;
const incTime = () => {
    currentTime += waitInterval;
    console.log(`waiting ${currentTime / 1000} seconds`);
    if (currentTime >= 5)  {
        clearInterval(interval);
    }
};

const interval = setInterval(incTime, waitInterval);
```

## Core Modules

To import modules in node.js we use the global function `require`.

```javascript
// This example import the node core module path with the require function and print the name of the current file.
const path = require('path');

path.basename(__filename);
```

Other core module is util, one the objects in the util module is log.

```javascript
const util = require('util');

util.log("Hello world.");
```

Other module is v8, that have access to some infos from the v8 engine.

```javascript
// This example prints info about the heap stats of v8 engine.
const v8 = require('v8');

console.log(v8.getHeapStatistics());
```

### The readline module

```javascript
// This codes uses the readline module to make questions in the standard input and output.
const readline = require('readline');

const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout,
});

rl.question('How do you like Node?', answer => {
    console.log(`Your answer: ${answer}`);
    rl.close();
});
```

### export modules

To export your files as a modules you can use the functions `module.exports` and `exports`, theses functions made accessible objects that can be exported from your module (file).

```javascript
module.exports = 'This is a module';
```

And then to export the last module

```javascript
// The filename is the name of from where we are exporting the module (Where the module is defined);
const text = require('./filename');
```

### EventEmitters

The EventEmitters is a node.js implementation of the Pub-Sub Design Patter and give us mechanism for emitting custom events and wiring up listeners and handlers for those events.

```javascript
// This example creates a customEvent and trigger the event twice.
const events = require('events');


const emitter = new events.EventEmitter();

emitter.on('customEvent', (message, user) => {
    console.log(`${user}: ${message}`);
});

emitter.emit('customEvent', 'Hello World', 'Computer');
emitter.emit('customEvent', 'That\'s pretty cool huh?', 'David');
```


## File System Basics

### List directory files

To list the content in a directory with the code below

```javascript
// This example read the list of files within a directory synchronously.
const fs = require('fs');

const files = fs.readdirSync('./directory');

console.log(files);
```

```javascript
// This example read the list of files within a directory asynchronously.
const fs = require('fs');

fs.readdir('./www/files/uploads', (err, files) => {
    if (err) {
        throw err;
    }
    
    console.log(files);
});
```

### Read files

You can read a file in node.js with the code below.

```javascript
// This example reads a file synchronously.
const fs =  require('fs');

// The param with the value 'utf-8' refers the file encoding.
const file = fs.readFileSync('./filename.txt', "utf-8");

console.log(file);
```

```javascript
// This example reads a file asynchronously.
const fs =  require('fs');

// The param with the value 'utf-8' refers the file encoding.
fs.readFile('./filename.txt', "utf-8", (err, text) => {
    if (err) {
        throw err;
    }

    console.log(text);
});
```

### Write and append files

With the fs module we can also write files.

```javascript
// This example write a file using fs module synchronously.
const fs = require('fs');

const md = `
# This is a new file

We cab write text to a file with fs.writeFile

* fs.readdir
* fs.readFile
* fs.writeFile
`;

fs.writeFileSync('./notes.md', md.trim());

console.log('file saved!');
```

```javascript
// This example write a file using fs module asynchronously.
const fs = require('fs');

const md = `
# This is a new file

We cab write text to a file with fs.writeFile

* fs.readdir
* fs.readFile
* fs.writeFile
`;

fs.writeFile('./notes.md', md.trim(), err => {
    if (err) {
        throw err;
    }

    console.log('file saved!');
});
```
### Create directories

We can also create directories with fs module

```javascript
// This examples checks if the directory exits and if not exits try to create the directory using the fs module in an synchronously.
const fs = require('fs');

if (fs.existsSync('assets')) {
    console.log('Directory already exits');
    process.exit();
}

fs.mkdirSync('assets');
console.log('directory created!');
```

```javascript
// This examples checks if the directory exits and if not exits try to create the directory using the fs module in an asynchronously.
const fs = require('fs');

if (fs.existsSync('assets')) {
    console.log('Directory already exits');
    process.exit();
}

fs.mkdir('assets', err => {
    if (err) {
        throw err;
    }

    console.log('directory created!');
});
```

### Append files

We can also modified files with node.js.

```javascript
/// This example append a file using fs module asynchronously.
const fs = require('fs');

const text = `
This new lines is just to say that node.js is a great tool!!
`;

fs.appendFile('notes.md', text, err => {
    if (err) {
        throw err;
    }

    console.log('The file appended successfully!');
});
```

### Rename and remove files
```javascript
const fs = require('fs');

fs.renameSync('./notes.md', './amazing-file.md');

fs.renameSync('./www/files/uploads/hello.txt', './logs/app.log', err => {
    if (err) {
        throw err;
    }

    console.log('File moved successfully!!');
});

setTimeout(() => {
    fs.unlinkSync('./www/files/uploads/File.md');
}, 4000);
```


## Files and Streams

The streams are interfaces that allows you stream data in Node.js using events.

### Readable Streams

The readable streams allows to read data from a stream in Node.js.

```javascript
// This example use fs module to create a readable stream to read a file.
const fs = require('fs');

// Here is created the stream
// utf-8 is the file encoding.
const readStream = fs.createReadStream('./filename', 'utf-8');

let fileText = "";

// Here we listen an event pretty like `process.stdin.on` method
// here we going to read the data in chucks to make the process
// more efficient.
readStream.on('data', data => {
    fileText += data;
    console.log(`I read ${data.length - 1} characters of text`);
});

// Here we listen a event but reads once, and just one chuck.
readStream.once('data', data => {
    console.log(data);
});


// Here we listen the end of the read of the stream.
readStream.on('end', () => {
    console.log('finished reading file');
    console.log(`In total I read ${fileText.length - 1} characters of text`);
});
```

### Writable file streams

We can also use writable to write data in a stream. For example `process.stdout` is a writable stream that means you can use write method.

```javascript
// This example creates a writable stream to create and write a 
// file from the text that user input in the terminal.
const fs = require('fs');

const writeStream = fs.createWriteStream('./filename', 'utf-8');

process.stdin.on('data', data => {
    writeStream.write(data);
});
```

### Child process with exec

Child process in Node.js are create to interact with other application within the Node.js app is hosted. The `exec` method is an asynchronous function.

```javascript
// This example uses child_process module to run dir command 
// should be also ls command in unix systems to list files
// in the current directory.
const cp = require('child_process');

cp.exec('dir', (err, data) => {
    if (err) {
        throw err;
    }

    console.log(data);
});
```

### Child process with spawn

```javascript
// This file is questions.js
const readline = require('readline');

const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

const questions = [
    "What is your name? ",
    "Where do you live? ",
    "What are you going to do with node.js? "
];

const collectAnswers = (questions, done) => {
    const answers = [];
    const [firstQuestion] = questions;

    const questionAnswered = answer => {
        answers.push(answer);
        if (answers.length < questions.length) {
            rl.question(questions[answers.length], questionAnswered);
        } else {
            done(answers);
        }
    };

    rl.question(firstQuestion, questionAnswered);
};

collectAnswers(questions, answers => {
    console.log('Thanks for your answers. ');
    console.log(answers);
    process.exit();
});

// This is the code to demonstrate the use of child processes with spawn.
// This could must to write in another file.
// This example executes the last code and interact with him
// using child_process.spawn
const cp = require('child_process');

const questionsApp = cp.spawn('node', ['questions.js']);


questionsApp.stdin.write('David \n');
questionsApp.stdin.write('Santo Domingo \n');
questionsApp.stdin.write('Get Money \n');

questionsApp.stdout.on('data', data => {
    console.log(`from the question app: ${data}`);
});

questionsApp.on('close', () => {
    console.log('questionApp process exited');
});
```
