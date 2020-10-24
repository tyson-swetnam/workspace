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

# Install QGIS

RUN apt update && apt install -y gnupg software-properties-common && \
    wget -qO - https://qgis.org/downloads/qgis-2020.gpg.key | gpg --no-default-keyring --keyring gnupg-ring:/etc/apt/trusted.gpg.d/qgis-archive.gpg --import && \
    chmod a+r /etc/apt/trusted.gpg.d/qgis-archive.gpg && \
    add-apt-repository "deb https://qgis.org/debian `lsb_release -c -s` main" && \
    apt update && apt install qgis qgis-plugin-grass

# set environment for PDAL & GDAL
ENV PROJ_LIB=/opt/conda/share/proj/
