Adapted from https://github.com/gbrindisi/wordpot by Gianluca Brindisi (with contributions from Jason Trost / ThreatStream and Larry W. Cashdollar).

## Changes in this version (wordpot-nx)

The original runs only on Python 2 (last commit e96889b, October 2018) and pins Flask 0.10.1. This version runs on Python 3 (tested with Python 3.11) and current Flask. Routes, templates, plugins, log messages and hpfeeds events are unchanged.

- Python 3 syntax: `print()`, `dict.items()`, `configparser`, `except ... as e`; plugin errors are logged with `str(e)` instead of `e.message`.
- `requirements.txt` pins Flask 3.1.3 and Werkzeug 3.1.9. The hpfeeds client is the Python 3 `hpfeeds` package (3.1.0) from PyPI, which keeps the API of threatstream/hpfeeds; it is only needed when `HPFEEDS_ENABLED = True`.
- The regex URL converter sets `part_isolating = False`, which Werkzeug 2.2 and later need for a converter that matches slashes; without it the `/wp-admin/...`, plugin and theme routes return 404.
- The admin handler redirects to `<site root>/wp-login.php` as an absolute URL, as the original did under Werkzeug 0.9 (current Werkzeug sends relative redirects as they are, which would send `/wp-admin/` to `/wp-admin/wp-login.php` and back in a loop).
- Route patterns are raw strings (no invalid escape warnings); `LOGGER.warn` is now `LOGGER.warning`.
- Optional environment variables: `WORDPOT_CONF` (configuration file, default `wordpot.conf` in the repository) and `WORDPOT_LOG` (log file, default `logs/wordpot.log`).

Install: `python3 -m venv venv && venv/bin/pip install -r requirements.txt && venv/bin/python wordpot.py --host 0.0.0.0 --port 80`

The original README follows.

---

# Wordpot

Wordpot is a Wordpress honeypot which detects probes for plugins, themes, timthumb and other common files used to fingerprint a wordpress installation.
   
    $ python wordpot.py --help
    Usage: wordpot.py [options]
    
    Options:
      -h, --help        show this help message and exit
      --host=HOST       Host address
      --port=PORT       Port address
      --title=TITLE     Blog Title
      --theme=THEME     Default theme name
      --plugins=PLUGINS Fake installed plugins
      --themes=THEMES   Fake installed plugins
      --ver=VERSION     Wordpress version

To configure the honeypot you can edit the config file `wordpot.conf` or provide arguments trough the command line interface as shown above.

## Theme support

You can use a wordpress theme as you would in a normal Wordpress installation by putting the theme folder in the `static/wp-content/themes/` directory. You might also need to edit the html skeleton which is stored in the `templates/` folder and should be named as your theme (e.g. `themename.html`) - take a look at `twentyeleven.html` to see how it works. 

To use the theme start wordpot with the theme option (default value is `twentyeleven`):
    
    $ python wordpot --theme=THEMENAME

Templates are built with the [Jinja2](http://jinja.pocoo.org) template engine.

## Expandable with plugins (*beta*)

Wordpot can be expanded with plugins to improve its behaviour or just expand some functionalities.

For example you can write a plugin to mimick a particular vulnerability in a Wordpress plugin or theme.

Learn more in the dedicated [wiki page](https://github.com/gbrindisi/wordpot/wiki/Plugins).

## Links

* Project Homepage: http://brindi.si/g/projects/wordpot.html

## License

ISC License.
 
> Copyright (c) 2012, Gianluca Brindisi < g@brindi.si >
>
> Permission to use, copy, modify, and/or distribute this software for any purpose with or without fee is hereby granted, provided that the above copyright notice and this permission notice appear in all copies.
>
> THE SOFTWARE IS PROVIDED "AS IS" AND THE AUTHOR DISCLAIMS ALL WARRANTIES WITH REGARD TO THIS SOFTWARE INCLUDING ALL IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS. IN NO EVENT SHALL THE AUTHOR BE LIABLE FOR ANY SPECIAL, DIRECT, INDIRECT, OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES WHATSOEVER RESULTING FROM LOSS OF USE, DATA OR PROFITS, WHETHER IN AN ACTION OF CONTRACT, NEGLIGENCE OR OTHER TORTIOUS ACTION, ARISING OUT OF OR IN CONNECTION WITH THE USE OR PERFORMANCE OF THIS SOFTWARE.
