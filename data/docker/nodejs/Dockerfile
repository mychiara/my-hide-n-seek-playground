FROM phusion/passenger-nodejs:0.9.19
MAINTAINER Andreas Ruck

# Set correct environment variables.
ENV HOME /root
ENV APP_BRANCH compose

# Use baseimage-docker's init process.
CMD ["/sbin/my_init"]

# remove default nginx site and activate built-in nginx
RUN rm /etc/nginx/sites-enabled/default && rm -f /etc/service/nginx/down

# cleanup after ourselves
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Make project dir and make webapp.conf available to nginx
RUN mkdir /home/app/webapp
ADD webapp.conf /etc/nginx/sites-enabled/webapp.conf

# Add runit script for app
COPY /scripts/run /etc/service/nodejs/run
COPY /scripts /scripts
RUN chmod +x /etc/service/nodejs/run /scripts/first_run.sh && \
  touch firstrun

# set npm registry and disable progress for speed
RUN npm config set registry https://registry.npmjs.org/ && npm set progress=false

# get app into container
RUN git clone https://github.com/mychiara/hns_app /home/app/webapp
RUN cd /home/app/webapp && git checkout $APP_BRANCH

# install dependendencies
RUN cd /home/app/webapp && npm i --silent --unsafe-perm --production