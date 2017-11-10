FROM ubuntu

# File Author
MAINTAINER Luis Alejandro Torres

# Port
EXPOSE 80

# Update the repository source list
RUN apt-get -y update && apt-get -y upgrade

# Install packages
RUN apt-get install -y apache2
RUN apt-get install -y python3.5 python3-pip
RUN rm -rf /var/lib/apt/lists/*


# Apache settings
RUN a2enmod xml2enc proxy proxy proxy_html proxy_http deflate
COPY ./000-default.conf /etc/apache2/sites-enabled/000-default.conf

# Manually set up the apache environment variables
ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_LOG_DIR /var/log/apache2
ENV APACHE_LOCK_DIR /var/lock/apache2
ENV APACHE_PID_FILE /var/run/apache2.pid

RUN mkdir -p $APACHE_RUN_DIR $APACHE_LOCK_DIR $APACHE_LOG_DIR

# Python
RUN python3.5 -m pip install pip --upgrade
RUN ln -s python3.5 /usr/bin/python

# Set the working directory to /home
WORKDIR /home

# Create run.py and settings.py files
RUN mkdir /home/eve
RUN touch /home/eve/run.py
RUN echo "from eve import Eve" >> /home/eve/run.py
RUN echo "app = Eve()" >> /home/eve/run.py
RUN echo "app.run()" >> /home/eve/run.py
RUN touch /home/eve/settings.py

# Install any needed packages specified in requirements.txt
RUN pip install Eve Flask PyMongo envparse

# Script
RUN touch start.sh
RUN echo "#!/bin/sh" >> start.sh
RUN echo "python /home/eve/run.py &" >> start.sh
RUN echo "/usr/sbin/apache2 -D FOREGROUND" >> start.sh

# Run Eve
CMD ["sh","start.sh"]
