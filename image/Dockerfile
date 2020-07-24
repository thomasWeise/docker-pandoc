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
FROM thomasweise/docker-texlive-thin
MAINTAINER Thomas Weise <tweise@hfuu.edu.cn>

RUN echo "Initial update." &&\
    apt-get update &&\
# install tzdata, which we will need down the line
    apt-get install -f -y --no-install-recommends tzdata &&\
    echo "Now installing cabal, imagemagick, and other tools." &&\
# We install cabal, a Haskell package manager, because we want the newest
# pandoc and filters which we can only get from there.
# We also install zlib1g, as we will need it later on.
# We install librsvg2 in order to make svg -> pdf conversation possible.
# imagemagick may be needed by the latex-formulae-pandoc filter
    apt-get install -f -y --no-install-recommends \
      cabal-install \
      imagemagick \
      librsvg2-bin \
      librsvg2-common \
      zlib1g \
      zlib1g-dev &&\
# fix the access rights for imagemagick
  echo "Fixing access rights for imagemagick." &&\
  sed -i -e 's/rights="none"/rights="read|write"/g' /etc/ImageMagick-6/policy.xml &&\
  sed -i -e 's/<\/policymap>/<policy domain="module" rights="read|write" pattern="{PS,PDF,XPS}" \/>\n<\/policymap>/g' /etc/ImageMagick-6/policy.xml &&\
# get the newest list of packages
    echo "Getting the newest list of cabal packages." &&\
    cabal update &&\
# install the dependencies of the packages we want
    echo "Installing depedencies of pandoc via cabal." &&\
    cabal install --dependencies-only \
                  pandoc &&\
    echo "Installing depedencies of pandoc-citeproc via cabal." &&\
    cabal install --dependencies-only \
                  pandoc-citeproc &&\
    echo "Installing depedencies of pandoc-citeproc-preamble via cabal." &&\
    cabal install --dependencies-only \
                  pandoc-citeproc-preamble &&\
    echo "Installing depedencies of pandoc-crossref via cabal." &&\
    cabal install --dependencies-only \
                  pandoc-crossref &&\
    echo "Installing depedencies of latex-formulae-pandoc via cabal." &&\
    cabal install --dependencies-only \
                  --allow-newer=base \
                  latex-formulae-pandoc &&\
# install the packages we want
    echo "Installing pandoc via cabal." &&\
    cabal install pandoc &&\
    echo "Installing pandoc-citeproc via cabal." &&\
    cabal install pandoc-citeproc &&\
    echo "Installing pandoc-citeproc-preamble via cabal." &&\
    cabal install pandoc-citeproc-preamble &&\
    echo "Installing pandoc-crossref via cabal." &&\
    cabal install pandoc-crossref &&\
    echo "Installing latex-formulae-pandoc via cabal." &&\
    cabal install latex-formulae-pandoc &&\
# clear unnecessary cabal files
    echo "Performing cleanup of unnecessary cabal files." &&\
    rm -rf /root/.cabal/logs &&\
    rm -rf /root/.cabal/packages &&\
    rm -rf /root/.cabal/lib &&\
    rm -rf /root/.cabal/share/doc &&\
    rm -rf /root/.cabal/share/man &&\
    find /root/.cabal/ -type f -empty -delete || true &&\
    find /root/.cabal/ -type d -empty -delete || true &&\
# clean up all temporary files
    echo "Performing more cleanup." &&\
    apt-get clean -y &&\
    rm -rf /var/lib/apt/lists/* &&\
    rm -f /etc/ssh/ssh_host_* &&\
# delete man pages and documentation
    echo "Deleting man pages and documentation." &&\
    rm -rf /usr/share/man &&\
    mkdir -p /usr/share/man &&\
    find /usr/share/doc -depth -type f ! -name copyright -delete &&\
    find /usr/share/doc -type f -name "*.pdf" -delete &&\
    find /usr/share/doc -type f -name "*.gz" -delete &&\
    find /usr/share/doc -type f -name "*.tex" -delete &&\
    find /usr/share/doc -type d -empty -delete || true &&\
    mkdir -p /usr/share/doc &&\
    rm -rf /var/cache/apt/archives &&\
    mkdir -p /var/cache/apt/archives &&\
    rm -rf /tmp/* /var/tmp/* &&\
    find /usr/share/ -type f -empty -delete || true &&\
    find /usr/share/ -type d -empty -delete || true &&\
    mkdir -p /usr/share/texmf/source &&\
    mkdir -p /usr/share/texlive/texmf-dist/source &&\
    echo "Done."

# we remember the path to pandoc in a special variable
ENV PANDOC_DIR=/root/.cabal/bin/

# add pandoc to the path
ENV PATH=${PATH}:${PANDOC_DIR}
