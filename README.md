leofs_docs
==========

Overview
--------

LeoFS uses [Sphinx](http://sphinx.pocoo.org/) for its documentation.
It can be consulted directly from the [LeoFS website](http://www.leofs.org/docs/).

The following commands can be used to download the documentation and
generate the HTML files:

```text
$ git clone https://github.com/leo-project/leofs_docs.git
$ cd leofs_docs
$ sudo easy_install cloud_sptheme
$ sudo easy_install sphinxcontrib-erlangdomain

## for Ubuntu/Debian
$ sudo apt-get install libjpeg8 libjpeg8-dev zlib1g-dev libfreetype6 libfreetype6-dev python-dev
$ sudo easy_install -UZ PIL reportlab rst2pdf

## for CentOS/RHEL
$ sudo yum install libjpeg libjpeg-devel freetype freetype-devel python-devel zlib-devel
$ sudo easy_install -UZ PIL reportlab rst2pdf

$ make html
```
