FROM tswetnam/workspace-gpu:latest 

# Install Geospatial dependencies and Google Earth Engine API
RUN conda update -n base conda && \
    conda install --quiet --yes -c conda-forge \
    altair \
    dask distributed \
    earthengine-api \
    entwine \
    fiona \    
    folium \
    humanize \
    gdal \
    geemap \
    google-api-python-client \
    ipyleaflet \
    ipyvolume \
    ipywidgets \
    nbdime \
    nodejs \ 
    oauth2client \
    palettable \
    pathlib2 \
    pdal \
    planet \
    proj \
    proj-data \
    pycrypto \
    rasterio \
    shapely \
    tiledb \
    udunits2 \
    vega_datasets \
    && \
    conda clean --all -f -y 

RUN jupyter labextension install \
    @bokeh/jupyter_bokeh \
    @jupyter-widgets/jupyterlab-manager \
    @jupyter-widgets/jupyterlab-sidecar \
    @jupyterlab/geojson-extension \
    @jupyterlab/toc \
    ipyvolume \
    itkwidgets \    
    jupyterlab_iframe \ 
    jupyter-leaflet \
    jupyter-threejs 

# theme stuff
RUN jupyter labextension install \
    jupyterlab-topbar-extension \
    jupyterlab-system-monitor \
    jupyterlab-theme-toggle --no-build 

RUN conda clean --all -f -y

# Install porder
RUN pip install porder

# Install CyberDuck CLI
RUN echo "deb https://s3.amazonaws.com/repo.deb.cyberduck.io stable main" | tee /etc/apt/sources.list.d/cyberduck.list > /dev/null && \
    apt-key adv --keyserver keyserver.ubuntu.com --recv-keys FE7097963FEFBE72 && \
    apt-get update && \ 
    apt-get install -y duck

RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Compile GRASS-GIS before QGIS

# data directory - not using the base images volume because then the permissions cannot be adapted
ENV DATA_DIR /data

# GRASS GIS compile dependencies
RUN apt-get update \
    && apt-get install -y --no-install-recommends --no-install-suggests \
        build-essential \
        libblas-dev \
        libbz2-dev \
        libcairo2-dev \
        libfftw3-dev \
        libfreetype6-dev \
        libgdal-dev \
        libgeos-dev \
        libglu1-mesa-dev \
        libgsl0-dev \
        libjpeg-dev \
        liblapack-dev \
        libncurses5-dev \
        libnetcdf-dev \
        libopenjp2-7 \
        libopenjp2-7-dev \
        libpdal-dev pdal \
        libpdal-plugin-python \
        libpng-dev \
        libpq-dev \
        libproj-dev \
        libreadline-dev \
        libsqlite3-dev \
        libtiff-dev \
        libxmu-dev \
        libzstd-dev \
        bison \
        flex \
        g++ \
        gettext \
        gdal-bin \
        language-pack-en-base \
        libfftw3-bin \
        make \
        ncurses-bin \
        netcdf-bin \
        proj-bin \
        proj-data \
        python3 \
        python3-dateutil \
        python3-dev \
        python3-numpy \
        python3-pil \
        python3-pip \
        python3-ply \
        python3-six \
        python3-wxgtk4.0 \
        python3-gdal \
        python3-matplotlib \
        sqlite3 \
        subversion \
        unixodbc-dev \
        zlib1g-dev \
    && apt-get autoremove \
    && apt-get clean && \
    mkdir -p $DATA_DIR

RUN echo LANG="en_US.UTF-8" > /etc/default/locale
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8

# Pull binary

RUN add-apt-repository ppa:ubuntugis/ubuntugis-unstable
RUN add-apt-repository ppa:grass/grass-devel
RUN apt-get update 
RUN apt-get install grass

# enable simple grass command regardless of version number
RUN ln -s /usr/local/bin/grass* /usr/local/bin/grass

# Reduce the image size
RUN apt-get autoremove -y
RUN apt-get clean -y

# set SHELL var to avoid /bin/sh fallback in interactive GRASS GIS sessions in docker
ENV SHELL /bin/bash

# Fix permissions
RUN chmod -R a+rwx $DATA_DIR

# declare data volume late so permissions apply
VOLUME $DATA_DIR
WORKDIR $DATA_DIR

# Further reduce the docker image size 
RUN rm -rf /code/grass

# Conda install wxpython required for GRASS GUI
RUN conda install -c anaconda wxpython

# Install QGIS

RUN apt-get update && apt-get install -y gnupg software-properties-common && \
    wget -qO - https://qgis.org/downloads/qgis-2020.gpg.key | gpg --no-default-keyring --keyring gnupg-ring:/etc/apt/trusted.gpg.d/qgis-archive.gpg --import && \
    chmod a+r /etc/apt/trusted.gpg.d/qgis-archive.gpg && \
    add-apt-repository "deb https://qgis.org/ubuntugis `lsb_release -c -s` main" 

RUN apt-get update && apt-get install -y qgis qgis-plugin-grass saga

