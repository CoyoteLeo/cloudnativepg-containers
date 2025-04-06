FROM ghcr.io/cloudnative-pg/postgresql:17-bookworm
ENV PG_MAJOR="17"

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

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        "make" \
        "gcc" \
        "libc-dev" \
        "postgresql-server-dev-${PG_MAJOR}" \
    && \
    wget https://github.com/EnterpriseDB/system_stats/archive/refs/tags/v3.2.tar.gz && \
    tar -zxvf v3.2.tar.gz && \
    cd system_stats-3.2 && \
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