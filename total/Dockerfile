# Download base image ubuntu 20.04 or use "latest" instead. See https://hub.docker.com/_/ubuntu
FROM ubuntu:20.04.1

# Describe person in charge of Dockerfile
MAINTAINER Luca l.valnegri@datamaps.co.uk
 
# Set constants
ARG PUB_GRP=public
ARG PUB_PATH=/usr/local/share/$PUB_GRP
ARG USERNAME=datamaps
ARG USER_UID=1000
ARG USER_GID=$USER_UID 

RUN 
    # Update software repository + Upgrade system
    apt update \
    # Avoid interactive keyboard requests
    && DEBIAN_FRONTEND=noninteractive TZ=Europe/London apt install -y tzdata \
    # Upgrade system
    && apt -y full-upgrade \
    # Install potentially missing basic commands
    && apt install -y --no-install-recommends \
        apt-utils \
	      apt-transport-https \
	      build-essential \
	      dos2unix \
	      git-core \
	      libgit2-dev \
	      libauthen-oath-perl \
	      libsocket6-perl \
	      man-db \
	      nano \
	      openssh-server \
	      software-properties-common \
    # Install R packages dependencies
    && apt-get install -y  \
        curl  \
        libcairo2-dev  \
        libcurl4-gnutls-dev  \
	      libssl-dev  \ 
	      libxml2-dev  \
        libxt-dev  \
	      pandoc  \
        pandoc-citeproc  \
        xtail \
    # cleaning
    && apt -y autoremove && apt clean  \
    && rm -rf /var/lib/apt/lists/  \
    && rm -rf /tmp/downloaded_packages/ /tmp/*.rds
    
RUN \
    # Create a new "public" group
    groupadd $PUB_GRP &&
    # Create a new directory to be used by the "public" group and connected with the similar host public dir
    mkdir -p $PUB_PATH/R_library  \
    && chgrp -R $PUB_GRP $PUB_PATH \
    && chmod -R 2775 $PUB_PATH
    
RUN \ 
    # add CRAN repository to apt
    echo -e "\n# CRAN REPOSITORY\ndeb https://cloud.r-project.org/bin/linux/ubuntu focal-cran40/\n" | tee -a /etc/apt/sources.list  \
    # add public key of CRAN maintainer
    && apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9  \
    # Update package manager
    && apt update  \
    # Install R
    && apt install -y r-base r-base-dev  \
    # Add configurations to .Rprofile
    echo '
	#####################################################
    	### ADDED BY DOCKERFILE
	PUB_PATH = '/usr/local/share/public'
	R_LIBS_USER = '/usr/local/share/public/R_library'
	R_MAX_NUM_DLLS = 1000
	#####################################################
    ' | tee -a $(R RHOME)/etc/Renviron \
    # Install devtools, shiny, and rmarkdown packages
    && su - -c "R -e \"install.packages(c('devtools', 'shiny', 'rmarkdown'), repos='https://cran.rstudio.com/')\""

RUN \
    # download and install RStudio Server
    wget -O rstudio.deb https://download2.rstudio.org/server/bionic/amd64/rstudio-server-1.4.1103-amd64.deb  \
    && sudo apt -y install ./rstudio.deb  \
    && rm rstudio.deb  \
    # download and install Shiny Server
    && wget -O shiny.deb https://download3.rstudio.org/ubuntu-14.04/x86_64/shiny-server-1.5.16.958-amd64.deb  \
    && sudo apt -y install ./shiny.deb  \
    && rm shiny.deb \
    # add "shiny"" to the "public" group
    && usermod -aG public shiny
    
# install R packages using R script (plus cleaning)
# RUN Rscript -e "install.packages()" \
#    && rm -rf /tmp/downloaded_packages/ /tmp/*.rds

RUN \
    # Create a new user datamaps
    useradd --create-home --home-dir /home/$USERNAME --no-log-init --shell /bin/bash --groups $PUB_GRP $USERNAME  \
    # add user and "public" group as owners of the shiny directory 
    && cd /srv/shiny-server \
    && chown -R $USERNAME:$PUB_GRP . \
    && chmod g+w . \ 
    && chmod g+s .

# Volume configuration
VOLUME ["/usr/local/share/public"]

# pass control to user "datamaps"
USER $USERNAME
WORKDIR /home/$USERNAME

