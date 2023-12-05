# Basic Node Package 101

## install Node.js

1. install nvm for node.js version management

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.6/install.sh | bash
```

```
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
```

2. install node.js for specific version

```
nvm install lts/iron
```

3. check node version

```
node -v
```

## create node package

1. create project folder

```
mkdir <name>say
cd <name>say
```

2. init node package (package.json will be generated)

```
npm init -y
```

3. create file `.npmrc` to config npm registry to default (incase default registry is other)

```
registry=https://registry.npmjs.org
```

4. install `yargs` lib for cli

```
npm install yargs
```

5. create bin folder and file for cli execution

```
mkdir bin & touch bin/say.js
```

6. edit file `bin/say.js`

```javascript
#! /usr/bin/env node

const fs = require("fs");
const yargs = require("yargs");

function readCharacter() {
  return fs.readFileSync(`${__dirname}/../character/bear.ascii`, "utf-8"); //read character text file
}

function say(word) {
  const character = readCharacter();
  const output = character
    .replace("$text", word) //replace $text with provided word
    .replaceAll("$bubble", "-".repeat(word.length * 1.2)); // replace $bubble with - to create bubble
  console.log(output); // print output to console
}

const command = yargs.argv.$0; // read command that use in command line
const args = yargs.argv._.join(" "); // join arguments in command

if (!args) {
  return console.log(`Usage: ${command} text`);
}

say(args);
```

7. find `ascii art` that you like

- https://www.asciiart.eu/
- https://ascii.co.uk/art
- https://www.textartcopy.com/simple-text-art.html

8. create character in text file ex. `bear.ascii` in folder `character`

```
   ▄▀▀▀▄▄▄▄▄▄▄▀▀▀▄      $bubble
   █▒▒░░░░░░░░░▒▒█     < $text
    █░░█░░░░░█░░█    /  $bubble
 ▄▄  █░░░▀█▀░░░█  ▄▄
█░░█ ▀▄░░░░░░░▄▀ █░░█
```

9. config add field `bin` and `files` in `package.json`

```
//package.json

"bin": {
    "<project-name>": "bin/say.js"
  },
"files": [
    "bin/",
    "character/"
  ],
```

10. login to npm registry type in terminal

```
npm login
```

11. publish node package to npm registry

```
npm publish
```

12. try your node package by typing in terminal

```
npx <your-project-name> hello world!!
```
