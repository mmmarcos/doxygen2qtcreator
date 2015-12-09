# doxygen2qtcreator

Integrate Doxygen-generated html docs into Qt Creator tooltips.

The doxygen2qtcreator.py scripts scans for documented classes inside Doxygen 'html' directory and it 
inserts markers used by Qt Creator to generate the tooltip when hovering over a class or method name.

It uses BeautifulSoup4 to parse and modify the html files.

## Usage

```
python doxy2qtcreator.py [-h] [-o OUTDIR] [-f FILE] [-q] [htmldir]

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

The script is rather simple and it currently handles Doxygen default html layout. However it should work if you only change the html layout (i.e. header or footer) but not the documentation structure (methods tables and divs).

You should use the `-o OUTDIR` to write the modified html files into another directory (specially if your documentation takes long time to re-generate).

```python doxy2qtcreator.py path_to_htmldir/ -o out/```

Tested with Doxygen 1.8.10 and Qt Creator 3.5.0.
