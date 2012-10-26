isBinaryFile
============

Detects if a file is binary in Node.js. Similar to [Perl's `-B` switch](http://stackoverflow.com/questions/899206/how-does-perl-know-a-file-is-binary), in that:

* it reads the first few thousand bytes of a file
* checks for a `null` byte; if it's found, it's binary
* flags non-ASCII characters. After a certain number of "weird" characters, the file is flagged as binary

It's also pretty much taken from [ag](https://github.com/ggreer/the_silver_searcher).

Note: please make sure the file exists before calling this function.

## Installation

```
npm install isBinaryFile
```

## Usage

If you pass in one argument, this module assumes it's just the file path, and performs the appropriate file read and stat functionality internally:

```javascript
var isBinaryFile = require("isbinaryfile");

if (isBinaryFile(process.argv[2]))
	console.log("It is!")
else
	console.log("No.")
```

Ta da. 


However, if you've already `stat()`-ed a file, you can pass in both the file's raw data and the stat's `size` info to save time:

```javascript
fs.readFile(process.argv[2], function(err, data) {
	fs.lstat(process.argv[2], function(err, stat) {
		if (isBinaryFile(data, stat))
			console.log("It is!")
		else
			console.log("No.")
	});
});
```