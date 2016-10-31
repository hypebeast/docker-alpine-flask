FROM sruml/alpine-python:2.7

MAINTAINER Sebastian Ruml <sebastian@sebastianruml.name>

ENV NODE_VERSION=v6.3.0 NPM_VERSION=3 FLASK_ENV=development FLASK_PORT=5000

RUN apk upgrade --update && \
    apk add --update nginx curl make gcc g++ linux-headers libgcc libstdc++ && \
    curl -sSL https://nodejs.org/dist/${NODE_VERSION}/node-${NODE_VERSION}.tar.gz | tar -xz && \
    cd /node-${NODE_VERSION} && \
    ./configure --prefix=/usr --without-snapshot && \
    make -j$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) && \
    make install && \
    cd / && \
    npm install -g npm@${NPM_VERSION} && \
    apk del gcc g++ linux-headers && \
    rm -rf /etc/ssl /node-${NODE_VERSION} /usr/include \
    /usr/share/man /tmp/* /var/cache/apk/* /root/.npm /root/.node-gyp \
    /usr/lib/node_modules/npm/man /usr/lib/node_modules/npm/doc /usr/lib/node_modules/npm/html \
    chown -R nginx:www-data /var/lib/nginx

# Expose the ports for nginx
EXPOSE 80 443

# Stream the nginx logs to stdout and stderr
RUN ln -sf /dev/stdout /var/log/nginx/access.log && \
    ln -sf /dev/stderr /var/log/nginx/error.log

# Install gunicorn
RUN pip install gunicorn

# Add the files
ADD ../root .

WORKDIR /app

ONBUILD RUN pip install -r requirements.txt
