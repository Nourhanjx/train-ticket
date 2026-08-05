FROM mirror.gcr.io/library/python:3.8

RUN /bin/cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo 'Asia/Shanghai' >/etc/timezone

RUN mkdir -p /app
WORKDIR /app

COPY requirements.txt ./
# ca-bundle.crt is optional (only present when building behind a TLS-intercepting proxy
# that pip needs to trust); the glob is a no-op COPY when the file doesn't exist.
COPY ca-bundle.cr[t] /tmp/
RUN if [ -f /tmp/ca-bundle.crt ]; then PIP_CERT=/tmp/ca-bundle.crt pip install cryptography; else pip install cryptography; fi
RUN if [ -f /tmp/ca-bundle.crt ]; then PIP_CERT=/tmp/ca-bundle.crt pip install --no-cache-dir -r requirements.txt; else pip install --no-cache-dir -r requirements.txt; fi

ADD ./server.py /app/
CMD [ "python", "server.py" ]

EXPOSE 16101