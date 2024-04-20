FROM ghcr.io/cloudnative-pg/postgresql:16.2-debian
ENV PG_MAJOR="16"

ENV TIMESCALEDB_MAJOR="2"

USER root

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        "lsb-release" \
        "wget" \
    && \
    apt-get clean && \
	rm -fr /tmp/* && \
	rm -rf /var/lib/apt/lists/*

# install timescaledb
RUN echo "deb https://packagecloud.io/timescale/timescaledb/debian/ $(lsb_release -c -s) main" | tee /etc/apt/sources.list.d/timescaledb.list && \
    wget --quiet -O - https://packagecloud.io/timescale/timescaledb/gpgkey | apt-key add - && \
    apt-get update && \
    apt-get install -y --no-install-recommends \
        "timescaledb-${TIMESCALEDB_MAJOR}-postgresql-${PG_MAJOR}" \
	&& \
    apt-get clean && \
	rm -fr /tmp/* && \
	rm -rf /var/lib/apt/lists/*

# installing Groonga
RUN apt-get update && \
    wget https://apache.jfrog.io/artifactory/arrow/debian/apache-arrow-apt-source-latest-$(lsb_release -c -s).deb && \
    apt-get install -y -V ./apache-arrow-apt-source-latest-$(lsb_release -c -s).deb && \
    rm apache-arrow-apt-source-latest-$(lsb_release -c -s).deb && \
    wget https://packages.groonga.org/debian/groonga-apt-source-latest-$(lsb_release -c -s).deb && \
    apt-get install -y -V ./groonga-apt-source-latest-$(lsb_release -c -s).deb && \
    rm groonga-apt-source-latest-$(lsb_release -c -s).deb && \
    apt-get install -y --no-install-recommends \
      "groonga-normalizer-mysql" \
      "groonga-token-filter-stem" \
      "groonga-tokenizer-mecab" \
     && \
    apt-get clean && \
	rm -fr /tmp/* && \
	rm -rf /var/lib/apt/lists/*;

# intsall PGroonga
RUN if [ "$(lsb_release -r -s)" -gt 11 ]; then \
    apt-get update && \
    apt-get install -y --no-install-recommends \
      "postgresql-${PG_MAJOR}-pgdg-pgroonga" \
    && \
    apt-get clean && \
	rm -fr /tmp/* && \
	rm -rf /var/lib/apt/lists/*; \
    fi

# intsall PGroonga for debian-bullseye, build from source
RUN if [ "$(lsb_release -r -s)" -le 11 ]; then \
    apt-get update && \
    apt-get install -y --no-install-recommends \
        "postgresql-server-dev-${PG_MAJOR}" \
        "libgroonga-dev" \
        "libmsgpack-dev" \
        "make" \
        "cmake" \
        "gcc" \
    && \
    pkg-config --list-all && \
    wget https://packages.groonga.org/source/pgroonga/pgroonga-latest.tar.gz && \
    tar xvf pgroonga-latest.tar.gz && \
    rm pgroonga-latest.tar.gz && \
    cd "$(ls | grep pgroonga)" && \
    make HAVE_MSGPACK=1 && \
    make install && \
    cd .. && \
    rm -rf "$(ls | grep pgroonga)" && \
    apt-get --purge autoremove -y \
        "postgresql-server-dev-${PG_MAJOR}" \
        "libgroonga-dev" \
        "libmsgpack-dev" \
        "make" \
        "cmake" \
        "gcc" \
    && \
    apt-get clean && \
	rm -fr /tmp/* && \
	rm -rf /var/lib/apt/lists/* ; \
    fi

RUN apt-get --purge autoremove -y \
        "lsb-release" \
        "wget" \
    && \
    apt-get clean && \
	rm -fr /tmp/* && \
	rm -rf /var/lib/apt/lists/*

USER postgres