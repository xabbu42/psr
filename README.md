Description
-----------

psr is a perl script inspired by [surfraw](http://surfraw.alioth.debian.org/). 
It of course misses a lot of features and supports not nearly as many searches as surfraw does. Its main advantage is the possibility to scrape search results out of the html page, using the *very* good [Web::Scraper](http://search.cpan.org/~miyagawa/Web-Scraper/lib/Web/Scraper.pm). This enables a "I'm feeling lucky" implementation working for all searches and makes it possible to list and use results directly in the shell.


Installation
------------

psr is easiest to install with [cpanminus](http://search.cpan.org/~miyagawa/App-cpanminus-1.1006/lib/App/cpanminus.pm):

    cpanm https://github.com/downloads/xabbu42/psr/App-psr-0.1.tar.gz

If you want to install psr manually you can use the following commands:

    wget --no-check-certificate https://github.com/downloads/xabbu42/psr/App-psr-0.1.tar.gz
    tar -xzf App-psr-0.1.tar.gz
    cd App-psr-0.1
    perl Makefile.PL
    make
    sudo make install


Usage
-----

See `psr -h` and `man psr` for complete documentation. Some examples:

    $ psr -l google test
    http://www.test.com/
    http://www.speakeasy.net/speedtest/
    http://en.wikipedia.org/wiki/Test_cricket
    http://www.bandwidthplace.com/
    http://www.humanmetrics.com/cgi-win/JTypes1.htm
    http://www.4degreez.com/misc/personality_disorder_test.mv
    http://test.org.uk/
    http://www.bbc.co.uk/science/humanbody/mind/surveys/smiles/
    http://acid3.acidtests.org/
    http://www.testing.com/

    $ psr -f google test #opens http://www.test.com in a browser

	$ psr -n 30 -u google test
    http://www.google.com/search?num=30&q=test

	$ psr -n 30 google test #opens http://www.google.com/search?num=30&q=test in a browser

Todos and Ideas
---------------

- Add support for a config file, especially to configure the browser command and to add new searches.

- Get more use out of [Web::Scraper](http://search.cpan.org/~miyagawa/Web-Scraper/lib/Web/Scraper.pm) and the scraping code coming with searches. 

  - Generalize --first to --follow NUM
  - Add title and/or descriptions of search results to output.
  - Open all results in tabs in one browser window
  - etc.

- Allow using stdin and POST method to support pastbins or github markdown preview.

- Document how to define searches.
