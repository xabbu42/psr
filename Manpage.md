# NAME

psr - perl surfraw

# SYNOPSIS

psr \[options\] \[provider\] \[query\]

# OPTIONS

- \`-h, --help\`

show brief help message

- \`--man\`

show complete man page

- \`-f, --first, --lucky\`

open first result (same as --open 1)

- \`-a, --all\`

open all results (same as --open 1-)

- \`-o, --open=RANGES\`

open this results (example "1-4,5,9-")

- \`-u, --url\`

print url(s) instead of opening browser

- \`-n, --num=NUM\`

number of results to get

- \`-l, --list\`

list results (same as --all --url)

- \`-p, --providers\`

list known providers

- \`--browser=CMD\`

use CMD to open a new browser

- \`-t, --tabs\`

open all urls with one browser command

- \`-w, --windows\`

open each url with seperate browser command

- \`-d, --dump\`

dump page to stdout

- \`-F, --file=FILE\`

take query from FILE instead of command line

- \`-r, --read=FILE\`

read web page from FILE instead of web

- \`-c, --config=FILE\`

read config file FILE with more providers \[~/.psrrc\]

- \`--configdir=DIR\`

read config files in directory DIR with more providers \[~/.psr\]

# DESCRIPTION

__This program__ is a perl replacement of surfraw. It generates search
urls and opens the url in a browser. You can optionally list the
results in your terminal or open the first result directly.

psr tries to use the default browser on most systems. You can
overwrite the used browser command by setting the environment variable
BROWSER or by giving an explicit browser argument.

# CONFIG FILES

Additional providers can be configured in the config file ~/.psrrc or
in one file per provider in the directory ~/.psr. All of this files
are in the [YAML](http://yaml.org) format.  ~/.psrrc should contain a
YAML hash with one hash value per additional provider under the
provider name as key. The files in ~/.psr should contain a YAML hash
with the additional provider.

The key `config` is reserved to override commandline option
defaults. So all the values in ~/.psr/config and under the `config`
key in ~/.psrrc override defaults for commandline options. Only the
long forms without the `--` of the commandline options are recognized
as keys.

A provider config hash can contain the following keys:

- url

The base url for the search.

- params

The parameters to add to the search url. The parameter values are perl
strings and the variables $query and $numresults can be used to get
the query and the number of results.

- method

The http method to use for the request. The default is `get` but
`post` is also supported.

- results

Some perl code to generate a [Web::Scraper](http://search.cpan.org/perldoc?Web::Scraper) object to scrape the
results from the returned html page. The scraper should return a list
of urls, one url for each search result.

- key (only in ~/.psr)

The key of the provider (the filename is used as default). This can be
a regular expression like `google|g` if different arguments
should match this provider.

As example, the following is the YAML configuration for the included
google search configuration:

    key:     google|g
    url:     http://www.google.com/search
    params:  {q: $query, num: $numresults},
    results: >
        scraper {process 'p > a', '[]' => sub {
            my $url = $_[0]->attr('href');
            my $q = URI->new($url)->query_param('q');
            return $q =~ m[^http://] ? $q : undef;
        }},

# AUTHOR

Nathan Gass <gass@search.ch>