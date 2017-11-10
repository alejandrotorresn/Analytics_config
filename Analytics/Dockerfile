from ubuntu

MAINTAINER Luis Alejandro Torres

# Port
EXPOSE 8888

# Ubuntu packages
RUN apt-get update --fix-missing
RUN apt-get install -y wget bzip2 git build-essential
RUN apt-get install -y libreadline6 libreadline6-dev inetutils-ping

# Create the home directory for the new Analytics user.
RUN mkdir -p /home/analytics/

# Create an Analytics user so our program doesn't run as root.
# RUN groupadd -r analytics && useradd -r -g analytics -d /home/analytics -s /sbin/nologin -c "Docker image user" analytics
# RUN usermod -a -G disk analytics

# Set the home directory to our analytics user's home.
ENV HOME=/home/analytics
ENV ANALYTICS_HOME=/home/analytics/Notebooks
RUN mkdir $ANALYTICS_HOME
WORKDIR $ANALYTICS_HOME

# Add Tini
ENV TINI_VERSION v0.16.1
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /usr/bin/tini
RUN chmod +x /usr/bin/tini
ENTRYPOINT ["/usr/bin/tini", "--"]

# Chown all the files to the app user.
# RUN chown -R analytics:analytics $HOME

# Set user
# USER analytics:analytics

# Install Miniconda
ENV REPO=https://repo.continuum.io/miniconda/Miniconda2-latest-Linux-x86_64.sh
RUN wget --quiet $REPO -O $HOME/anaconda.sh
RUN /bin/bash $HOME/anaconda.sh -b -p $HOME/conda
RUN echo 'export PATH=$HOME/conda/bin:$PATH' >> $HOME/.bashrc
RUN rm $HOME/anaconda.sh
ENV PATH $HOME/conda/bin:$PATH

# Update Conda and pip
RUN conda update conda -y
RUN conda update python -y

# Install packages
RUN conda install bokeh numpy matplotlib pandas scikit-learn scikit-image jupyter jupyterlab ipywidgets numba pyproj scipy seaborn sqlite pymongo tensorflow zlib
RUN pip install pymc3 motionless utm envparse

# Clear packages conda
RUN conda clean --tarballs -y

# Run Jupyter
CMD ["jupyter-lab", "--ip=0.0.0.0", "--port=8888", "--no-browser", "--allow-root", "--NotebookApp.token="]
