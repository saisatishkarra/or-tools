# Create a virtual environment with all tools installed
# ref: https://hub.docker.com/_/ubuntu
FROM ubuntu

# Install system build dependencies
RUN apt-get update -qq \
&& apt-get install -yq git wget curl

ARG REMOTE_CACHE_SEVER
RUN curl http://${REMOTE_CACHE_SEVER}:8080/
# RUN bazel build --curses=no --cxxopt=-std=c++20 --copt='-Wno-sign-compare' --remote_cache=http://localhost:8080 //ortools/... //examples/...