FROM python:3.6-alpine

ARG http_proxy
ARG https_proxy
ARG no_proxy
ARG PYPI_URL
ARG PYPI_HOST

RUN apk update && \
    apk add gcc libffi-dev musl-dev

WORKDIR /jup

RUN set -ex && [ -z "${PYPI_URL}" ] || pip_args=" --index-url ${PYPI_URL} " ; \
    [ -z "$PYPI_HOST" ] || pip_args="$pip_args --trusted-host $PYPI_HOST " ; \
    echo "$no_proxy" |tr ',' '\n' | sort -u |grep "^$PYPI_HOST$" || \
    [ -z "$http_proxy" ] || pip_args="$pip_args --proxy $http_proxy " ; 


RUN pip install $pip_args --upgrade pip && \
    pip install $pip_args jupyter && \
    pip install $pip_args jupyterlab

EXPOSE 8888

ENTRYPOINT ["jupyter", "lab", "--ip=0.0.0.0", "--allow-root", "--no-browser"]