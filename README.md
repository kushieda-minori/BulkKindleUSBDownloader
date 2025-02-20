# Bulk Kindle USB Downloader


This is a fork of [BulkKindleUSBDownloader](https://github.com/Jedi425/BulkKindleUSBDownloader), itself a fork of
[bOOkp](https://github.com/sghctoma/bOOkp). (Thanks, Jedi425 and sghtoma!)

My (bellisk's) updates:
- support downloading books from amazon.co.uk, amazon.com, amazon.de and amazon.it
- download books that you have access to because they're shared with you by a family member, as well as books you
  have purchased

This is a quick'n'dirty script to download all your Kindle ebooks.

I needed to backup all my Kindle e-books, so I put together this script. It does
work for now, but a change in the download process will probably break it, and I
may not have the time to fix it right away.

You can download all your e-books (that are eligible for download), or you can
specify multiple ASINs to download. By default the script will only display
warnings, errors, and a finish message. If you want to see progress, you have to
use the `--verbose` flag. Selenium with ChromeDriver is used to handle login,
and you can display the browser with `--showbrowser` - this may come handy if
something goes wrong.

You can also output logging to a file with `--logfile`; this is needed if you have 
enough books to fill your terminal buffer.

The only mandatory command line parameter is the e-mail address associated with
your Amazon account, but of course the script will need your password too - it
will ask for it if not given as parameter. Keep in mind that passwords given as
parameters will probably be stored in you history!

The script will also ask which of your devices you want to download your books
to. This is important, because the downloaded books will be DRMd to that
particular device. The serial number (which is required to remove DRM) will be
printed (and saved to the log if specified) when the books are downloaded.

## Tips

If the script doesn't log in successfully, wait a moment and try again. Sometimes, 
the Amazon website doesn't load correctly, and the script can't find the 'sign in' 
button to click. It will then fail with a 'selenium.common.exceptions.NoSuchElementException' 
error.

I recommend using the LOGFILE variable to save your output, it will note the ASINs of 
any books it fails to download, so you can fill those in with a second pass or else
manually download them.

If running on MacOS you must use the --showbrowser option, as xvfb isn't supported, however with this option the script will work.

## Usage

1. Install requirements:
   ```shell
   pip install -r requirements.txt
   ```
   If the requirements install fails, you may need to run in a python virtual environment. You can do that with the following commands on Linux, OSX or (WSL):
   ```shell
   python3 -m venv .venv
   source .venv/bin/activate
   pip install -r requirements.txt
   ```
   On Linux or (WSL), you might also need to install `xvfb`, e.g.:
   ```shell
   sudo apt-get install xvfb
   ```
2. Then:
   ```shell
   ./bookp.py --email {YOUR_EMAIL}
   ```
   If you needed to use the virtual environment and left it, you can re-enter it by running the following before running `bookp.py`
   ```shell
   python3 -m venv .venv
   source .venv/bin/activate
   ```
   See below for more options. Your email address is the only mandatory command-line parameter.
3. The script will prompt you for:
   - your password, if not given as parameter
   - your Amazon OATH code (optional)
   - the Amazon country site that manages your e-books: currently available are UK, USA, Germany, Italy and Canada.
     If you need a different Amazon site, you can edit `bookp.py` to add it to the `environment` array.
   - the device that you want to download the books for

Full options for the script:

```
usage: ./bookp.py [-h] [--verbose] [--showbrowser] --email EMAIL
                [--password PASSWORD] [--outputdir OUTPUTDIR] [--proxy PROXY]
                [--oath OATH] [--asin [ASIN [ASIN ...]]] [--logfile FILENAME]

Amazon e-book downloader.

optional arguments:
  -h, --help            show this help message and exit
  --verbose             show info messages
  --showbrowser         display browser while creating session.
  --email EMAIL         Amazon account e-mail address
  --password PASSWORD   Amazon account password
  --oath OATH           Amazon one-time pass
  --outputdir OUTPUTDIR
                        download directory (default: books)
  --proxy PROXY         HTTP proxy server
  --asin [ASIN [ASIN ...]]
                        list of ASINs to download
  --logfile FILENAME    log output to a file
```

## Troubleshooting

If you are having issues with getting a working session, it may be due to Amazon displaying a captcha challenge. Use the `--showbrowser` option to see the chrome session. You will then have about 15 seconds to enter the captcha challenge.

## Requirements

* [Python 3.x](https://www.python.org)
* [ChromeDriver](https://sites.google.com/a/chromium.org/chromedriver/downloads)
* the following Python modules:
  * [requests](https://pypi.org/project/requests/)
  * [PyVirtualDisplay](https://pypi.org/project/PyVirtualDisplay/)
  * [selenium](https://pypi.org/project/selenium/)
