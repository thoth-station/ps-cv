FROM quay.io/thoth-station/s2i-generic-data-science-notebook:v0.0.4
LABEL name="ps-cv-pytorch:latest" \
      summary="computer vision Source-to-Image for Python 3.8 applications." \
      description="Notebook image based on Source-to-Image.These images can be used in OpenDatahub JupterHub." \
      io.k8s.description="Notebook image based on Source-to-Image.These images can be used in OpenDatahub JupterHub." \
      io.k8s.display-name="computer vision Notebook Python 3.8-ubi8 S2I" \
      io.openshift.expose-services="8888:http" \
      io.openshift.tags="python,python38" \
      authoritative-source-url="https://quay.io/thoth-station/ps-cv-pytorch" \
      io.openshift.s2i.build.commit.ref="master" \
      io.openshift.s2i.build.source-location="https://github/thoth-station/ps-cv-pytorch" \
      io.openshift.s2i.build.image="quay.io/thoth-station/s2i-generic-data-science-notebook:v0.0.4"

ENV THAMOS_RUNTIME_ENVIRONMENT="ps-cv-pytorch" \
    THOTH_ADVISE="0" \
    THOTH_ERROR_FALLBACK="1" \
    THOTH_DRY_RUN="1" \
    THAMOS_DEBUG="0" \
    THAMOS_VERBOSE="1" \
    THOTH_PROVENANCE_CHECK="0" \
    JUPYTER_ENABLE_LAB="true" \
    ENABLE_MICROPIPENV="1" \
    UPGRADE_PIP_TO_LATEST="1"

USER root

# Install binary needed for pytesseract
RUN dnf install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm

# Install Leptonica from Source
RUN  wget -q http://www.leptonica.org/source/leptonica-1.76.0.tar.gz -O leptonica-1.76.0.tar.gz
RUN tar -xzf leptonica-1.76.0.tar.gz --no-same-owner -C /opt/app-root/ &&\
  cd /opt/app-root/leptonica-1.76.0 &&\
  ./autobuild &&\
  ./configure &&\
  make &&\
  make install &&\
  cd

# Install binary needed for open-cv
RUN dnf install -y mesa-libGL

# Copying in override assemble/run scripts
COPY .s2i/bin /tmp/scripts
# Copying in source code
COPY . /tmp/src
# Change file ownership to the assemble user. Builder image must support chown command.
RUN chown -R 1001:0 /tmp/scripts /tmp/src
USER 1001
RUN /tmp/scripts/assemble
CMD /tmp/scripts/run
