ARG CODE_VERSION=latest

FROM postgres:${CODE_VERSION}

ARG TARGET_LOCALE=en_US
ARG TARGET_ENCODING=UTF-8

RUN apt-get update && apt-get install -y \
    locales \
    && rm -rf /var/lib/apt/lists/*

RUN localedef -i ${TARGET_LOCALE} -c -f ${TARGET_ENCODING} -A /usr/share/locale/locale.alias ${TARGET_LOCALE}.${TARGET_ENCODING}

# Set locale
ENV LANG=${TARGET_LOCALE}.${TARGET_ENCODING}
ENV LC_ALL=${TARGET_LOCALE}.${TARGET_ENCODING}
