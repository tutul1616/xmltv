# XMLTV 0.6.1

## Table of Contents
- [XMLTV 0.6.1](#xmltv-061)
  * [Description](#description)
  * [Changes](#changes)
  * [Installation](#installation)
    + [Required distribtions/modules](#required-distribtions-modules)
    + [Recommended distribtions/modules](#recommended-distribtions-modules)
    + [JSON libraries](#json-libraries)
    + [CPAN Shell](#cpan-shell)
    + [Proxy servers](#proxy-servers)
  * [Known issues](#known-issues)
  * [Author and copying](#author-and-copying)
  * [Resources](#resources)

## Description

Gather television listings, process them and organize your viewing.
XMLTV is a file format for storing TV listings, defined in xmltv.dtd.
Then there are several tools to produce and process these listings.

Please see [QuickStart](doc/QuickStart) for documentation on what each program does, and xmltv.dtd for documentation on the file format.

## Changes

For a list of major changes on the current release (0.6.1) please consult our [Changes](Changes) file.

## Installation

Note: Windows users are strongly advised to use the pre-built EXE as installing all the prerequisites is non-trivial. For those who want to give it a go, instructions are in [doc/exe_build.html](doc/exe_build.html). Those instructions can be used for both building xmltv.exe as well as a local install.

Basic installation instructions (Linux/Unix):

```bash
% perl Makefile.PL
% make
% make test
% make install
```

To install in a custom directory, replace the first line with
something like

```
% perl Makefile.PL PREFIX=/wherever/
```

The system requirements are Perl 5.8.3 or later, and a few Perl modules. You will be asked about some optional components; if you choose not to install them then there are fewer dependencies.

Please note that in addition to the specific modules listed below, the
`tv_grab_zz_sdjson_sqlite` grabber requires Perl 5.16 to be installed.

### Required distribtions/modules

Required distributions/modules for XMLTV's core libraries are:

```perl
Date::Manip 5.42a
File::Slurp
JSON (see note below)
LWP 5.65
Term::ReadKey
XML::LibXML
XML::Parser 2.34
XML::TreePP
XML::Twig 3.28
XML::Writer 0.6.0
```

Required modules for grabbers/utilities are:

```
Archive::Zip                  (tv_grab_eu_epgdata, tv_grab_uk_bleb)
CGI                           (tv_pick_cgi, core module until 5.20.3, part of CGI)
CGI::Carp                     (tv_pick_cgi, core module until 5.20.3, part of CGI)
Compress::Zlib                (for some of the grabbers, core module since 5.9.3, part of IO::Compress)
Data::Dump                    (for tv_grab_it_dvb)
Date::Calc                    (tv_grab_il)
Date::Format                  (for some of the grabbers, part of TimeDate)
Date::Language                (tv_grab_ar, part of TimeDate)
DateTime                      (for several of the grabbers)
DateTime::Format::ISO8601     (tv_grab_zz_sdjson_sqlite)
DateTime::Format::SQLite      (tv_grab_zz_sdjson_sqlite)
DateTime::Format::Strptime    (tv_grab_eu_epgdata)
DateTime::TimeZone            (tv_grab_fr)
DBD::SQLite                   (tv_grab_zz_sdjson_sqlite)
DBI                           (tv_grab_zz_sdjson_sqlite)
Digest::SHA                   (tv_grab_zz_sdjson{,_sqlite}, core module since 5.9.3)
File::HomeDir                 (tv_grab_zz_sdjson_sqlite)
File::Which                   (tv_grab_zz_sdjson_sqlite)
HTML::Entities 1.27           (for several of the grabbers, part of HTML::Parser 3.34)
HTML::Parser 3.34             (tv_grab_it, tv_grab_it_dvb, part of HTML::Parser 3.34)
HTML::Tree                    (for many of the grabbers, part of HTML::Tree)
HTML::TreeBuilder             (for many of the grabbers, part of HTML::Tree)
HTTP::Cache::Transparent 1.0  (for several of the grabbers)
HTTP::Cookies                 (for several of the grabbers)
HTTP::Request::Common         (tv_grab_eu_epgdata, part of HTTP::Message)
IO::Scalar                    (for some of the grabbers, part of IO::Stringy)
List::MoreUtils               (tv_grab_zz_sdjson_sqlite)
LWP::Protocol::https          (tv_grab_fi, tv_grab_huro, tv_grab_zz_sdjson)
LWP::UserAgent::Determined    (tv_grab_zz_sdjson_sqlite)
SOAP::Lite 0.67               (tv_grab_na_dd)
Time::Piece                   (tv_grab_huro, core module since 5.9.5)
Time::Seconds                 (tv_grab_huro, core module since 5.9.5)
Tk                            (tv_check)
Tk::TableMatrix               (tv_check)
URI                           (for some of the grabbers, part of URI)
URI::Escape                   (for some of the grabbers, part of URI)
XML::DOM                      (tv_grab_is)
XML::LibXSLT                  (tv_grab_is)
```

### Recommended distribtions/modules

The following modules are recommended (e.g. faster JSON processing, better character handling) but the software will works without them installed:

```
File::chdir                      (testing grabbers)
JSON::XS                         (faster JSON handling, see note below)
Lingua::Preferred 0.2.4          (helps with multilingual listings)
Log::TraceMessages               (useful for debugging, not needed for normal use)
PerlIO::gzip                     (can make tv_imdb a bit faster)
Term::ProgressBar                (displays pretty progress bars)
Unicode::String                  (improved character handling in tv_to_latex)
```

### JSON libraries

By default, libraries and grabbers that need to handle JSON data should specify the JSON module. This module is a wrapper for JSON::XS-compatible modules and supports the following JSON modules:

```
JSON::XS
JSON::PP
Cpanel::JSON::XS
```

JSON will use JSON::XS if available, falling back to JSON::PP (a core module since 5.14.0) if JSON::XS is not available. Cpanel::JSON::XS can be used as an explicit alternative by setting the PERL_JSON_BACKEND environment variable
(please refer to the JSON module's documentation for details).

### CPAN Shell

All required modules can can be installed from CPAN using the CPAN shell program:

```bash
% 'perl -MCPAN -e shell'
```

then `install XML::Twig` and so on.

You may find it easier to search for packaged versions of modules from your OS provider - software sources which distribute a packaged version of XMLTV will often provide the modules it needs too.

### Proxy servers

Proxy server support is provide by the LWP modules.
You can define a proxy server via the HTTP_PROXY enviornment variable.
    `http_proxy=http://somehost.somedomain:port`

For more information, see the this [article](http://search.cpan.org/~gaas/libwww-perl-5.803/lib/LWP/UserAgent.pm#$ua->env_proxy)

## Known issues

If a full HTTP URL to the XMLTV.dtd is provided in the DOCTYPE declaration of an XMLTV document, be aware that it is possible for the link to instead redirect to a page for accepting cookies. Such cookie-acceptance pages are more common in Europe, and can result in applications being unable to parse the file.

## Author and copying

This is free software distributed under the GPL, see COPYING. There are many who have contributed code: they are credited in individual source files and in the [authors](authors.txt) mapping file.

## Resources

We have a project [web page and wiki](http://www.xmltv.org)

We run the following mailing lists:

    - xmltv-announce: Subscribe — XMLTV Release Announcements (low traffic)

    - `xmltv-users`: — XMLTV users list, mostly for problem reporting and general XMLTV questions

    - `xmltv-devel`: Subscribe — XMLTV development discussion group

Please subscribe to any/all lists at https://sourceforge.net/p/xmltv/mailman/

Finally, we run an IRC channel #xmltv on Freenode. Please join us!

-- Nick Morrott, knowledgejunkie@gmail.com, 2019-02-21
