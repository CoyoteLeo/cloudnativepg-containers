FROM ghcr.io/cloudnative-pg/postgresql:16.2-debian
ENV PG_MAJOR="16"

ENV PGROONGA_VERSION="3.2.0-1"
ENV TIMESCALEDB_MAJOR="2"

USER root

# install timescaledb
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        "lsb-release" \
        "wget" \
	&& \
    echo "deb https://packagecloud.io/timescale/timescaledb/debian/ $(lsb_release -c -s) main" | tee /etc/apt/sources.list.d/timescaledb.list && \
    wget --quiet -O - https://packagecloud.io/timescale/timescaledb/gpgkey | apt-key add - && \
    apt-get update && \
    apt-get install -y --no-install-recommends \
        "timescaledb-${TIMESCALEDB_MAJOR}-postgresql-${PG_MAJOR}" \
	&& \
    apt-get clean && \
	rm -fr /tmp/* && \
	rm -rf /var/lib/apt/lists/*

# extra dependencies for installing PGroonga on debian-bookworm
RUN if [ "$(lsb_release -c -s)" = "bookworm" ]; then \
    apt-get update && \
    apt-get install -y --no-install-recommends \
        "lsb-release" \
        "wget" \
    && \
    wget https://apache.jfrog.io/artifactory/arrow/debian/apache-arrow-apt-source-latest-$(lsb_release -c -s).deb && \
    apt-get install -y -V ./apache-arrow-apt-source-latest-$(lsb_release -c -s).deb && \
    rm apache-arrow-apt-source-latest-$(lsb_release -c -s).deb; \
    fi


# intsall PGroonga for debian-bullseye
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        "lsb-release" \
        "wget" \
    && \
    wget https://packages.groonga.org/debian/groonga-apt-source-latest-$(lsb_release -c -s).deb && \
    apt-get install -y -V ./groonga-apt-source-latest-$(lsb_release -c -s).deb && \
    rm groonga-apt-source-latest-$(lsb_release -c -s).deb && \
    apt-get update && \
    apt-get install -y --no-install-recommends \
      groonga-normalizer-mysql \
      groonga-token-filter-stem \
      groonga-tokenizer-mecab && \
    apt-get clean && \
	rm -fr /tmp/* && \
	rm -rf /var/lib/apt/lists/*

USER postgres