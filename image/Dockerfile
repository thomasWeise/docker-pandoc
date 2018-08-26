#
# thomasweise/docker-pandoc
#
# This is an image with a pandoc installation with several filters
# which can make use of a full texlive installation with tools and
# fonts.
# Source: https://github.com/thomasWeise/docker-pandoc/
# License: GNU GENERAL PUBLIC LICENSE, Version 3, 29 June 2007
# The license applies to the way the image is built, while the
# software components inside the image are under the respective
# licenses chosen by their respective copyright holders.
#
FROM thomasweise/docker-texlive-full
MAINTAINER Thomas Weise <tweise@hfuu.edu.cn>

RUN apt-get clean &&\
    apt-get update &&\
    apt-get autoclean -y &&\
    apt-get autoremove -y &&\
    apt-get update &&\
# install tzdata, which we will need down the line
    apt-get install -f -y tzdata &&\
# install zlib1g, as we will need it later on
    apt-get install -f -y zlib1g \
                          zlib1g-dev &&\
# we install cabal, a Haskell package manager, because we want the newest
# pandoc and filters which we can only get from there
    apt-get install -f -y cabal-install &&\
# get the newest list of packages
    cabal update &&\
# install the dependencies of the packages we want
    cabal install --dependencies-only \
                  pandoc \
                  pandoc-citeproc \
                  pandoc-crossref \
                  latex-formulae-pandoc &&\
# install the packages we want
    cabal install pandoc \
                  pandoc-citeproc \
                  pandoc-crossref \
                  latex-formulae-pandoc &&\
# install in order to make svg -> pdf conversation possible
    apt-get install -f -y librsvg2-bin \
                          librsvg2-common &&\
# clean up all temporary files
    apt-get clean &&\
    apt-get autoclean -y &&\
    apt-get autoremove -y &&\
    apt-get clean &&\
    rm -rf /tmp/* /var/tmp/* &&\
    rm -rf /var/lib/apt/lists/* &&\
    rm -f /etc/ssh/ssh_host_*

# add pandoc to the path
ENV PATH=${PATH}:/root/.cabal/bin/
