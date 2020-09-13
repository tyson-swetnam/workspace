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

# Install GRASS-GIS

RUN apt-get update && sudo apt-get upgrade -y

# install PROJ
RUN apt-get install libproj-dev proj-data proj-bin -y

# optionally, install (selected) datum grid files

RUN cd /tmp && \
    wget https://download.osgeo.org/proj/proj-datumgrid-world-latest.zip && \
    wget https://download.osgeo.org/proj/proj-datumgrid-oceania-latest.zip && \
    wget https://download.osgeo.org/proj/proj-datumgrid-north-america-latest.zip && \
    wget https://download.osgeo.org/proj/proj-datumgrid-europe-latest.zip && \
    cd /usr/share/proj/ && \
    for datumfile in $(ls /tmp/proj-datumgrid-*-latest.zip) ; do ** \
    unzip $datumfile && rm -f $datumfile ** \
    done

# install GEOS
RUN apt-get install libgeos-dev -y

# install GDAL
RUN apt-get install libgdal-dev python3-gdal gdal-bin -y

# install PDAL (optional)
RUN apt-get install libpdal-dev pdal libpdal-plugin-python -y

# recommended to give Python3 precedence over Python2 (which is end-of-life since 2019)
RUN update-alternatives --install /usr/bin/python python /usr/bin/python3 1

# install SAGA-GIS

RUN apt update && apt-get install -y saga

# Install QGIS

RUN apt update && apt install -y gnupg software-properties-common && \
    wget -qO - https://qgis.org/downloads/qgis-2020.gpg.key | gpg --no-default-keyring --keyring gnupg-ring:/etc/apt/trusted.gpg.d/qgis-archive.gpg --import && \
    chmod a+r /etc/apt/trusted.gpg.d/qgis-archive.gpg && \
    add-apt-repository "deb https://qgis.org/debian `lsb_release -c -s` main" && \
    apt update && apt install qgis qgis-plugin-grass

