![](./img/Kipper-Logo-with-head.png)

# Kipper Base Package - `@kipper/base`

[![Version](https://img.shields.io/npm/v/@kipper/base?label=release&color=%23cd2620&logo=npm)](https://npmjs.org/package/@kipper/base)
[![FOSSA Status](https://app.fossa.com/api/projects/git%2Bgithub.com%2FLuna-Klatzer%2FKipper.svg?type=shield)](https://app.fossa.com/projects/git%2Bgithub.com%2FLuna-Klatzer%2FKipper?ref=badge_shield)
![](https://img.shields.io/badge/Coverage-73%25-5A7302.svg?style=flat&logoColor=white&color=blue&prefix=$coverage$)
[![Issues](https://img.shields.io/github/issues/Luna-Klatzer/Kipper)](https://github.com/Luna-Klatzer/Kipper/issues)
[![License](https://img.shields.io/github/license/Para-Lang/Para?color=cyan)](https://github.com/Luna-Klatzer/Kipper/blob/main/LICENSE)
[![Install size](https://packagephobia.com/badge?p=@kipper/base)](https://packagephobia.com/result?p=@kipper/base)
[![Publish size](https://badgen.net/packagephobia/publish/@kipper/base)](https://packagephobia.com/result?p=@kipper/base)

The base module and dependency for the entire Kipper project, which contains the core language and compiler.

Kipper is a simple TS-based strongly and statically typed programming language, which is designed to allow for
simple and straightforward coding similar to TypeScript and Python.

*Note that this is a development version! Stable releases might take until April/May 2022*

## Docs

For proper documentation on the kipper language go [here](https://wmc-ahif-2021.github.io/Kipper-Web/)!

*This is a project in work, and as such some docs pages can be work in progress!*

## Usage

To use, Kipper you have three options:
- Run it in the browser using the CDN `kipper-standalone.min.js` file, which bundles the entire compiler
  for your browser.
- Run it using the NodeJS CLI
- Import it as a NodeJS package in your code

### In a browser

For running Kipper in the browser, you will have to include the `kipper-standalone.min.js` file, which
provides the kipper compiler for the browser. This script will fetch your code from script tags with
the property `type="text/kipper"`, and replace the content with runnable JavaScript.

As a dependency you will also have to include `babel.min.js`, which is needed to allow for a compilation
from TS to JS in your browser, as Kipper compiles only to TypeScript.

Simple example of running your code in your browser using Kipper and Babel:

```html
<!-- Babel dependency -->
<script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
<!-- Kipper dependency -->
<script src="https://cdn.jsdelivr.net/npm/@kipper/base@latest/kipper-standalone.min.js"></script>

<!-- You won't have to define Kipper or anything after including the previous file. It will be defined per default  -->
<!-- with the global 'Kipper' -->
<script type="module">
  // Define your own logger and compiler, which will handle the compilation
  const logger = new Kipper.KipperLogger((level, msg) => {
    console.log(`[${Kipper.getLogLevelString(level)}] ${msg}`);
  });
  // Define your own compiler with your wanted configuration
  const compiler = new Kipper.KipperCompiler(logger);
  
  // Compile the code to Typescript
  // Top-level await ref: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await#top_level_await
  const result = (await compiler.compile(`call print("Hello world!");`)).write();
  
  // Transpile the TS code into JS
  const jsCode = Babel.transform(result, { filename: "kipper-web-script.ts", presets: ["env", "typescript"] });
  
  // Finally, run your program
  eval(jsCode.code);
</script>
```

### Locally using Node.js and the CLI

This is to recommend way to use Kipper if you want to dive deeper into Kipper, as it allows you to locally use and run 
kipper, without depending on a browser. 

This also enables the usage of files, which can be read and compiled to TypeScript, and run using the CLI, without
having to do anything yourself. This also allows the input of data over the console and file-interactions, 
which are not supported inside a browser.

For more info go to the CLI repository [here](https://github.con/Luna-Klatzer/Kipper-CLI) or visit the npm page 
[here](https://www.npmjs.com/package/@kipper/cli).

### Locally in your own code as a package

This is the recommended way if you intend to use kipper in a workflow or write code yourself to manage 
the compiler. This also allows for special handling of logging and customising the compilation process.

Simple example of using kipper using Node.js:

```ts
import * as ts from "typescript";
import { promises as fs } from "fs";
import { KipperCompiler } from "@kipper/base";

const path = "INSERT_PATH";
fs.readFile(path, "utf8" as BufferEncoding).then(
  (fileContent: string) => {
    // Define your own logger and compiler, which will handle the compilation
    const yourLogger = new Kipper.KipperLogger((level, msg) => {
      console.log(`[${level}] ${msg}`);
    })
    const yourCompiler = new Kipper.KipperCompiler(yourLogger);
    
    // Compile the code string or stream
    let code = yourCompiler.compile(fileContent).write();
      
    // Compiling down to JS using the typescript node module
    let jsCode = ts.transpile(code);
    
    // Running the Kipper program
    eval(jsCode);
  }
);
```


## License
[![FOSSA Status](https://app.fossa.com/api/projects/git%2Bgithub.com%2FLuna-Klatzer%2FKipper.svg?type=large)](https://app.fossa.com/projects/git%2Bgithub.com%2FLuna-Klatzer%2FKipper?ref=badge_large)