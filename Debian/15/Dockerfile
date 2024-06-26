FROM ghcr.io/cloudnative-pg/postgresql:15.6-debian
ENV PG_MAJOR="15"

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

# Install extra dependencies for installing PGroonga in debian-bookworm
RUN if [ "$(lsb_release -c -s)" = "bookworm" ]; then \
    wget https://apache.jfrog.io/artifactory/arrow/debian/apache-arrow-apt-source-latest-$(lsb_release -c -s).deb && \
    apt-get install -y -V ./apache-arrow-apt-source-latest-$(lsb_release -c -s).deb && \
    rm apache-arrow-apt-source-latest-$(lsb_release -c -s).deb && \
    apt-get clean && \
	rm -fr /tmp/* && \
	rm -rf /var/lib/apt/lists/*; \
    fi

# intsall PGroonga
RUN wget https://packages.groonga.org/debian/groonga-apt-source-latest-$(lsb_release -c -s).deb && \
    apt-get install -y -V ./groonga-apt-source-latest-$(lsb_release -c -s).deb && \
    rm groonga-apt-source-latest-$(lsb_release -c -s).deb && \
    apt-get update && \
    apt-get install -y --no-install-recommends \
      "postgresql-${PG_MAJOR}-pgdg-pgroonga" \
      "groonga-normalizer-mysql" \
      "groonga-token-filter-stem" \
      "groonga-tokenizer-mecab" \
    && \
    apt-get clean && \
	rm -fr /tmp/* && \
	rm -rf /var/lib/apt/lists/*

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        "make" \
        "gcc" \
        "libc-dev" \
        "postgresql-server-dev-${PG_MAJOR}" \
    && \
    wget https://github.com/EnterpriseDB/system_stats/archive/refs/tags/v2.1.tar.gz && \
    tar -zxvf v2.1.tar.gz && \
    cd system_stats-2.1 && \
    PATH="/usr/local/pgsql/bin:$PATH" make USE_PGXS=1 && \
    PATH="/usr/local/pgsql/bin:$PATH" make install USE_PGXS=1 && \
    apt-get --purge autoremove -y \
        "make" \
        "gcc" \
        "libc-dev" \
        "postgresql-server-dev-${PG_MAJOR}" \
    && \
    apt-get clean && \
	rm -fr /tmp/* && \
	rm -rf /var/lib/apt/lists/*

USER postgres