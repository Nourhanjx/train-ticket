FROM mirror.gcr.io/library/python:3.8

RUN /bin/cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo 'Asia/Shanghai' >/etc/timezone

# apt (deb.debian.org) is unreachable from this build environment, so get cmake (needed
# to build dlib) from PyPI instead, and use opencv-python-headless so we don't need the
# apt-only libgl1-mesa-glx runtime dependency.
# ca-bundle.crt is optional (only present when building behind a TLS-intercepting proxy
# that pip needs to trust); the glob is a no-op COPY when the file doesn't exist.
COPY ca-bundle.cr[t] /tmp/
RUN if [ -f /tmp/ca-bundle.crt ]; then CMAKE_POLICY_VERSION_MINIMUM=3.5 PIP_CERT=/tmp/ca-bundle.crt pip install cmake; else CMAKE_POLICY_VERSION_MINIMUM=3.5 pip install cmake; fi

ENV PYTHONUNBUFFERED=TRUE

RUN mkdir -p /app
ADD . /app/
WORKDIR /app

RUN if [ -f /tmp/ca-bundle.crt ]; then CMAKE_POLICY_VERSION_MINIMUM=3.5 PIP_CERT=/tmp/ca-bundle.crt pip install -r requirements.txt; else CMAKE_POLICY_VERSION_MINIMUM=3.5 pip install -r requirements.txt; fi

CMD ["gunicorn", "-w", "4", "-b", "0.0.0.0:17001", "app:app"]

EXPOSE 17001