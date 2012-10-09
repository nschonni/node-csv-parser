---
language: en
layout: page
title: "Writing data to a destination"
date: 2012-10-02T15:35:24.009Z
comments: false
sharing: false
footer: false
navigation: csv
github: https://github.com/wdavidw/node-csv
---


The `csv().to` property provide convenient functions to write 
to a csv output like a stream or a file. You may call
the `to` function or one of its sub function. For example, here is
to identical way to read to a file:

```javascript

csv.from(data).to('/tmp/data.csv');
csv.from(data).to.path('/tmp/data.csv');
```


<a name="to"></a>`to(mixed)`
-----------

Write from any sort of destination. A convenient function to discover the 
destination. If is an function, then the csv will be provided as the first 
argument of the callback. If it is a string, then it is expected to be a 
file path. If it is an instance of stream, it consider the object to be an  
output stream. 

Here's some examples on how to use this function

```javascript

csv()
.from('"1","2","3","4","5"')
.to(function(data){ console.log(data) })

csv()
.from('"1","2","3","4","5"')
.to('./path/to/file.csv')

csv()
.from('"1","2","3","4","5"')
.to(fs.createWriteStream('./path/to/file.csv'))
```



<a name="to.options"></a>`to.options([options])`
-----------------------

Update and retrieve options relative to the output. Return the options 
as an object if no argument is provided.

*   `delimiter`   Set the field delimiter, one character only, defaults to `options.from.delimiter` which is a comma.
*   `quote`       Defaults to the quote read option.
*   `quoted`      Boolean, default to false, quote all the fields even if not required.
*   `escape`      Defaults to the escape read option.
*   `columns`     List of fields, applied when `transform` returns an object, order matters, see the transform and the columns sections below.
*   `header`      Display the column names on the first line if the columns option is provided.
*   `lineBreaks`  String used to delimit record rows or a special value; special values are 'auto', 'unix', 'mac', 'windows', 'unicode'; defaults to 'auto' (discovered in source or 'unix' if no source is specified).
*   `flags`       Defaults to 'w', 'w' to create or overwrite an file, 'a' to append to a file. Applied when using the `toPath` method.
*   `newColumns`  If the `columns` option is not specified (which means columns will be taken from the reader options, will automatically append new columns if they are added during <a name="transform"></a>`transform()`.
*   `end`         Prevent calling `end` on the destination, so that destination is no longer writable, similar to passing `{end: false}` option in <a name="stream.pipe"></a>`stream.pipe()`.


<a name="to.string"></a>`to.string(callback, [options])`
------------------------------

Provide the output string to a callback.

```javascript

csv()
.from(input)
.to(function(ouput){
  console.log(ouput);
}, options)
```



<a name="to.stream"></a>`to.stream(stream, [options])`
------------------------------

Write to a stream. Take a writable stream as first argument and  
optionally an object of options as a second argument.


<a name="to.path"></a>`to.path(path, [options])`
--------------------------

Write to a path. Take a file path as first argument and optionally an object of 
options as a second argument. The `close` event is sent after the file is written. 
Relying on the `end` event is incorrect because it is sent when parsing is done 
but before the file is written.
