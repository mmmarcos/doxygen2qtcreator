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
When running Doxygen, this will generate an `index.qhp` file in your `html` directory and execute `qhelpgenerator` to compile it into Qt Help format (the `my_docs.qch` file). You can then import this file into Qt Creator (Tools/Options/Help/Documentation) to have your docs available using F1.

The problem is that you wont get your docs brief's inside Qt Creator tooltips if you hover over a class method or variable.

## What this script does
 
Qt Creator uses special markers to retrieve class/method brief from html docs. The doxygen2qtcreator.py looks for documented classes in your Doxygen `html` directory and inserts these markers (html comment tags) at the begining and ending of a class/method brief documentation.

When compiling your docs, Qt Creator will find these markers and include your docs in the tooltips.

## How to use it

After running Doxygen, you need to run the script specifying the path to the `html` directory : 
```
python doxygen2qtcreator.py HTMLDIR
```

You can then use Qt's qhelpgenerator tool to compile to Qt Help format :
```
QTDIR/bin/qhelpgenerator HTMLDIR/index.qhp -o my_docs.qch
```

And finally you can import `my_docs.qch` into Qt Creator. 

*Notice that you may disable the tags `QCH_FILE` and `QHG_LOCATION` in your Doxyfile since you will be generating the QCH file yourself*.

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
