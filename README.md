# fi - Smart Contract language for Tezos

fi (pronounced fee) is a powerful, smart contract language for Tezos that compiles down to valid and verifiable Michelson code. fi is currently in early alpha stages - please use at your own risk, and validate any compiled code.

[Check us out online](https://fi-code.com), or read through our [Documentation](https://learn.fi-code.com/) to get started.

## Installation

### Node.js

You can install the npm plugin:

```npm install fi-compiler```

You can the include the installed module:

```javascript
var fi = require("fi-compiler");
```

### Web

You can also build a distributable version to be included on the web:

```npm run build```

This will generate a file within the dist directory. You can download and use the currently built dist file as well.

```html
<script src="fi-compile.min.js"></script>
```

This will expose a global function, fi:

```javascript
console.log(fi);
```

### CLI

You can also install our npm CLI plugin, which allows you to compile fi directly from the command line:

```npm i -g fi-cli```

You can then compile code directly from your terminal using:

```bash
fi compile test_contract.fi
```
This will output two files in the same directory as the target fi file:
xx.fi.abi => The contract ABI
xx.fi.ml => The compuled michelson code


## Usage

You can use the following exposed functions when using the web or node.js library:

### fi.version

This returns the current version number of the fi-compiler.

```console.log(fi.version);```

### fi.compile(code, config)

This takes one input argument, a string containing fi compile. This returns an object with two elements, code and abi.

```javascript
var ficode = `
storage string name;
entry changeName(string name ){
  storage.name = input.name;
}
`;
var compiled = fi.compile(ficode);
console.log(compiled.ml); //Michelson code
console.log(compiled.abi); //ABI JSON string
```

You can also parse a config object, which is optional. The default settings are:

- ml_format - defaults to "optimized" for minimal michelson, can set as "readable" for a human readable version, or "array" for a JS array
- abi_format - defaults to "optimized" for the bare minimum, can be set as "full" for a full ABI copy including an array representation of the fi code
- macros - WIP

### fi.abi.load(abi)

You can manually loan an ABI string, which can be used to build input parameters for contract calls. In future, we aim to add additional support for more helper functions.

```javascript
fi.abi.load(compiled.abi);
```

### fi.abi.entry(entry, input)

This function allows you to convert a JSON input object into packed bytes for calling a contract using a loaded ABI file. This function takes two input arguments, the name of the function you are calling, and the JSON object with the input.

```javascript
var input = {
  name : "Stephen"
};
console.log(fbi.abi.entry("changeName", input)); // Returns packed bytes for a contract call
```
```
