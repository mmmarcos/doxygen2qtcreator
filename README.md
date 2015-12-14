# doxygen2qtcreator

Integrate Doxygen-generated html documentation into Qt Creator tooltips.

## Context

If you use Qt Creator IDE and Doxygen, you can include the documentation of your API into Qt Creator. You need to set the following tags in your Doxyfile : 
```
GENERATE_QHP       = YES
QHP_NAMESPACE      = "my_namespace"
QCH_FILE           = "path/to/my_docs.qch"
QHG_LOCATION       = "path/to/qhelpgenerator"
```
When running Doxygen, this will generate an `index.qhp` file in your `html` directory and execute `qhelpgenerator` to compile it into Qt Help format (the `my_docs.qch` file). You can then import this file from Qt Creator / Tools / Options / Help / Documentation. Your docs will be available using F1.

The problem is that you wont get descriptive tooltips if you hover over a class method or variable. Qt Creator won't show your class or method description insde tooltips.

## What this script does
 
The doxygen2qtcreator.py is a simple script that looks for documented classes in your Doxygen `html` directory and inserts special markers (in the form of html comment tags) used by Qt Creator to identify the begining and ending of a class/method brief documentation (among other things).

This will allow you to integrate your classes and method brief into Qt Creator tooltips.

## How to use it

After compiling your docs with Doxygen, you need to run the script specifying the path to the `html` directory : 
```
python doxygen2qtcreator.py path/to/html
```

You can then use Qt's qhelpgenerator tool to compile to Qt Help format :
```
QTPATH/bin/qhelpgenerator index.qhp -o my_docs.qch
```

And finally you can import `my_docs.qch` into Qt Creator. 

NOTE: You should notice that you may disable the tags QCH_FILE and QHG_LOCATION in your Doxyfile since you will be generating the QCH file yourself.

## Usage

```
python doxygen2qtcreator.py [-h] [-o OUTDIR] [-f FILE] [-q] [htmldir]

positional arguments:
  htmldir               Doxygen html directory (defaults to current directory)

optional arguments:
  -h, --help            show this help message and exit
  -o OUTDIR, --outdir OUTDIR
                        output directoy (defaults to htmldir)
  -f FILE, --file FILE  process only the specified file(s) (htmldir will be
                        ignored)
  -q, --quiet           decreases output verbosity (shows only errors)
```

## Disclaimer

The script serves it purpose but it is not ~~heavily~~ tested. I've only tested it with Doxygen's default html layout. 

Feel free to contact me if you have ideas on how to improve it.

Tested with Doxygen 1.8.10 and Qt Creator 3.5.0.
