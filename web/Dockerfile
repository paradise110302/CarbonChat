# Python Flask app example
FROM heroku/heroku:18

ARG GIT_SHA=dev
RUN apt-get -qq update && \
    apt-get install -y \
        curl \
        python3 \
        python3-pip \
        bash && \
        curl https://cli-assets.heroku.com/install-ubuntu.sh | sh && \
        pip3 --version && \
        python3 --version && \
        heroku --version

# setup Python libs
ADD ./app/requirements.txt /tmp/requirements.txt
RUN pip3 install --no-cache-dir -q -r /tmp/requirements.txt

# User account to use
RUN useradd -m carbonchat
RUN mkdir -p /opt/carbonchat && \
    chown -R carbonchat /opt/carbonchat && \
    chmod -R 744 /opt/carbonchat

USER carbonchat

ADD app /opt/carbonchat
WORKDIR /opt/carbonchat

# setup gitsha
RUN echo "$GIT_SHA" > /opt/carbonchat/gitsha.txt

# EXPOSE 5000
# default should be 5000
CMD gunicorn --bind 0.0.0.0:$PORT wsgi 