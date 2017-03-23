FROM fluent/fluentd:v0.12-onbuild
MAINTAINER Aravind Shankar <aravind@hasura.io>
USER root
WORKDIR /home/fluent
ENV PATH /home/fluent/.gem/ruby/2.3.0/bin:$PATH

COPY fluent-plugin-out-http/fluent-plugin-out-http-0.2.0.gem /home/fluent/
RUN apk --no-cache --update add \
                            build-base \
                            ruby-dev && \
    echo 'gem: --no-document' >> /etc/gemrc && \
    gem install fluent-plugin-secure-forward && \
    gem install fluent-plugin-record-reformer && \
    gem install fluent-plugin-kubernetes_metadata_filter && \
    gem install ./fluent-plugin-out-http-0.2.0.gem && \
    apk del build-base ruby-dev && \
    rm -rf /tmp/* /var/tmp/* /var/cache/apk/* /usr/lib/ruby/gems/*/cache/*.gem

# Copy configuration files
COPY ./conf/fluent.conf /fluentd/etc/
COPY ./conf/kubernetes.conf /fluentd/etc/

# Environment variables
ENV FLUENTD_OPT=""
ENV FLUENTD_CONF="fluent.conf"
#ENV LD_PRELOAD="/usr/lib/libjemalloc.so.2"

# Run Fluentd
CMD exec fluentd -c /fluentd/etc/$FLUENTD_CONF -p /fluentd/plugins $FLUENTD_OPT
