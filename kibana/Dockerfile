FROM debian:jessie

# add our user and group first to make sure their IDs get assigned consistently
RUN groupadd -r kibana && useradd -r -m -g kibana kibana

RUN apt-get update && apt-get install -y \
		ca-certificates \
		wget \
	--no-install-recommends && rm -rf /var/lib/apt/lists/*

# https://www.elastic.co/guide/en/kibana/4.4/setup.html#kibana-apt
# https://packages.elasticsearch.org/GPG-KEY-elasticsearch
RUN apt-key adv --keyserver ha.pool.sks-keyservers.net --recv-keys 46095ACC8548582C1A2699A9D27D666CD88E42B4

ENV KIBANA_MAJOR 4.5
ENV KIBANA_VERSION 4.5.1

RUN echo "deb http://packages.elastic.co/kibana/${KIBANA_MAJOR}/debian stable main" > /etc/apt/sources.list.d/kibana.list

RUN set -x \
	&& apt-get update \
	&& apt-get install -y --no-install-recommends kibana=$KIBANA_VERSION \
	&& chown -R kibana:kibana /opt/kibana \
	&& rm -rf /var/lib/apt/lists/*

# Uncomment the following line out to change the top left icon to the solute logo.
COPY logo-salute1.svg /opt/kibana/optimize/bundles/src/ui/public/images/kibana.svg

ENV PATH /opt/kibana/bin:$PATH

COPY docker-entrypoint.sh /

EXPOSE 5601

RUN chmod -R og+w /opt/kibana
USER 1000

ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["kibana"]
