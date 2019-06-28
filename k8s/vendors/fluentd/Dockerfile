FROM fluent/fluentd:v1.5.1-debian-1.0

# Use root account to use apt
USER root

# below RUN includes plugin as examples elasticsearch is not required
# you may customize including plugins as you wish
RUN buildDeps="sudo make gcc g++ libc-dev ruby-dev libpq-dev" \
 && apt-get update \
 && apt-get install -y --no-install-recommends $buildDeps

RUN fluent-gem install fluent-plugin-sql --no-document
RUN fluent-gem install pg -v 0.21.0 --no-document
RUN fluent-gem install fluent-plugin-s3 -v 1.0.0 --no-document
RUN fluent-gem install fluent-plugin-multi-format-parser -v 1.0.0 --no-document
RUN fluent-gem install fluent-plugin-rewrite-tag-filter -v 1.0.0 --no-document
RUN fluent-gem install fluent-plugin-route -v 1.0.0 --no-document

RUN sudo gem sources --clear-all \
 && SUDO_FORCE_REMOVE=yes \
    apt-get purge -y --auto-remove \
                  -o APT::AutoRemove::RecommendsImportant=false \
                  $buildDeps \
 && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/lib/ruby/gems/*/cache/*.gem

EXPOSE 8080

USER fluent
