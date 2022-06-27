# Create a virtual environment with all tools installed
# ref: https://hub.docker.com/_/ubuntu
FROM ubuntu:rolling AS env
LABEL maintainer="corentinl@google.com"

ENV TZ=Europe/Paris
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Install system build dependencies
RUN apt-get update -qq \
&& apt-get install -yq git wget curl libssl-dev build-essential \
&& apt-get install -yq python3-dev python3-pip \
&& apt-get install -yq default-jdk \
&& apt-get clean \
&& rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
# Install Bazel
RUN curl https://bazel.build/bazel-release.pub.gpg | apt-key add -
RUN echo "deb [arch=amd64] https://storage.googleapis.com/bazel-apt stable jdk1.8" | tee /etc/apt/sources.list.d/bazel.list
RUN apt-get update -qq \
&& apt-get install -yq bazel \
&& apt-get clean \
&& rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

FROM env AS devel
WORKDIR /home/project
COPY . .

FROM devel AS build
ARG REMOTE_CACHE_SEVER
RUN bazel version
# RUN curl http://${REMOTE_CACHE_SEVER}:8080/status
RUN bazel build --curses=no --cxxopt=-std=c++20 --copt='-Wno-sign-compare' --remote_cache=http://${REMOTE_CACHE_SEVER}:8080 //ortools/... //examples/...

FROM build AS test
ARG REMOTE_CACHE_SEVER
RUN bazel test -c opt --curses=no --cxxopt=-std=c++20 --copt='-Wno-sign-compare' --remote_cache=http://${REMOTE_CACHE_SEVER}:8080 //ortools/... //examples/...
