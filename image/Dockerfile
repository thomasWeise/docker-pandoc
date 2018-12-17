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
# We install cabal, a Haskell package manager, because we want the newest
# pandoc and filters which we can only get from there.
# We also install zlib1g, as we will need it later on.
# We install librsvg2 in order to make svg -> pdf conversation possible.
# imagemagick may be needed by the latex-formulae-pandoc filter
    apt-get install -f -y cabal-install \
                          imagemagick \
                          librsvg2-bin \
                          librsvg2-common \
                          zlib1g \
                          zlib1g-dev &&\
# fix the access rights for imagemagick
  sed -i -e 's/rights="none"/rights="read|write"/g' /etc/ImageMagick-6/policy.xml &&\
  sed -i -e 's/<\/policymap>/<policy domain="module" rights="read|write" pattern="{PS,PDF,XPS}" \/>\n<\/policymap>/g' /etc/ImageMagick-6/policy.xml &&\
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
# clear unnecessary cabal files
    rm -rf /root/.cabal/logs &&\
    rm -rf /root/.cabal/packages &&\
# clean up all temporary files
    apt-get clean &&\
    apt-get autoclean -y &&\
    apt-get autoremove -y &&\
    apt-get clean &&\
    rm -rf /tmp/* /var/tmp/* &&\
    rm -rf /var/lib/apt/lists/* &&\
    rm -f /etc/ssh/ssh_host_*

# we remember the path to pandoc in a special variable
ENV PANDOC_DIR=/root/.cabal/bin/

# add pandoc to the path
ENV PATH=${PATH}:${PANDOC_DIR}
