# DotNetZip


## Overview

The `DotNetZip` class allows one to zip and unzip files with .NET Core on all major platforms: Windows, Linux and Mac OS.

Note that although .NET Core is required on Linux and Mac OS, under Windows it also works with .NET rather than .Net Core.


## Requirements

Dyalog APL: 

* Under Windows DotNetZip runs under 17.0 and later
* Under Linux and MacOS .NET Core is required which is supported only by Dyalog 18.0 and later

.NET: 

* Under Windows any modern version of .NET **_or_** .NET Core 3.1 (or better) is required

* On non-Windows platforms .NET Core 3.1 (or better) is required


## Using shared methods

There are three shared methods available:

* `ZipFolder` 
* `UnzipTo`
* `ListZipContents`

The names should tell the whole story.

Note that when you pass a path like `C:\Temp\Foo` to `ZipFolder` and that folder contains:

```
readme.txt
sub/foo.dll
```

then the files in the ZIP files carry only the names `readme.txt` and `sub/foo.dll` rather than the full path.


## Using instance methods

First you need to create an instance. The constructor requires the name of the ZIP file to be created (if it does not exists yet) or altered (in case it already exists):

```
 myZip←DotNetZip (,⊂'C:\Temp\MyZip')
```

The instance offers these methods:

```
Add             
Delete          
Dispose 
ExtractTo
List            
Update          
```


### Processing one or more files

`Add`, `Delete`, ExtractTo` and `Update` all process these right arguments:

* A simple character vector as a single filename 
* A vector of text vectors, which is treated as a vector of filenames (and in case of `ExtractTo`, also folder names)


### Relative versus absolute paths with instances

If you want to save these files:

```
foo/readme.txt
foo/sub/foo.dll
```

with an instance of `DotNetZip` then there are two different scenarios:

#### Relative to the current directory

When the folder `foo` lives in your current directory then, assuming that your instance of `DotNetZip` is named `myZip`, you can just specify:

```
      myZip.Add 'foo/readme.txt' 'f00/sub/foo.dll'
```

#### Use an absolute path and preserve it

When the folder `foo` lives in `/tmp/MyStuff/` then you could use this:

```
      myZip.Add '/tmp/MyStuff/foo/readme.txt' 
      myZip.Add '/tmp/MyStuff/foo/sub/foo.dll'
```

The obvious disadvantage is that the full path is preserved in the ZIP file, meaning that the files can only ever be unzipped into `/tmp/MyStuff/`; if that does not exist, the operation will fail.    


### Specifying a parent folder

To get around this you can specify a parent folder as left argument, and specify on the right everything that should be preserved, `foo/` in our case:

```
      '/tmp/MyStuff/' myZip.Add 'foo/readme.txt' 
      '/tmp/MyStuff/' myZip.Add 'foo/sub/foo.dll'
```

Note that it does not matter whether the left argument carries a trailing separator or not, and that it does not matter whether the right argument(s) carry a leading separator or not: `DotNetZip` will work that out.
