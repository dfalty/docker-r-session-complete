FROM rstudio/r-session-base:bionic-1.2.1320-1

ARG R_VERSION=3.5.3
ARG PLATFORM=ubuntu-1804
ARG PRO_DRIVERS_VERSION=7C152C12

# Install system dependencies
RUN apt-get update -qq && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y \
    apt-transport-https \
    build-essential \
    curl \
    gfortran \
    libatlas-base-dev \
    libbz2-dev \
    libcurl4-openssl-dev \
    libicu-dev \
    liblzma-dev \
    libpcre3-dev \
    locales \
    sudo \
    libtcl8.6 \
    libtk8.6 \
    tzdata \
    wget \
    zlib1g-dev

# Install additional dependencies
RUN apt-get update -y && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y \
    git \
    libssl-dev \
    libssl1.0.0 \
    libuser \
    libxml2-dev \
    locales \
    pandoc \
    subversion

# Install R
RUN wget https://cdn.rstudio.com/r/${PLATFORM}/R-${R_VERSION}-${PLATFORM}.tar.gz && \
    mkdir -p /opt/R && \
    tar zx -C /opt/R -f ./R-${R_VERSION}-${PLATFORM}.tar.gz && \
    rm R-${R_VERSION}-${PLATFORM}.tar.gz

# Install R packages
RUN /opt/R/${R_VERSION}/bin/R -e 'install.packages("devtools", repos="http://cran.us.r-project.org")' && \
    /opt/R/${R_VERSION}/bin/R -e 'install.packages("tidyverse", repos="http://cran.us.r-project.org")' && \
    /opt/R/${R_VERSION}/bin/R -e 'install.packages("shiny", repos="http://cran.us.r-project.org")' && \
    /opt/R/${R_VERSION}/bin/R -e 'install.packages("rmarkdown", repos="http://cran.us.r-project.org")'

# Include R in PATH
ENV PATH="/opt/R/${R_VERSION}/bin:${PATH}"

# Install RStudio Professional Drivers
RUN apt-get update -y && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y unixodbc unixodbc-dev
RUN /opt/R/${R_VERSION}/bin/R -e 'install.packages("odbc", repos="http://cran.us.r-project.org")'
RUN mkdir -p /opt/rstudio-drivers && \
    cd /opt/rstudio-drivers && \
    wget https://drivers.rstudio.org/${PRO_DRIVERS_VERSION}/odbc-install.sh && \
    bash odbc-install.sh --eula

# Set the locale
RUN locale-gen en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8