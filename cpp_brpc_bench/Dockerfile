FROM gcc:12.2.0

RUN apt-get update && apt-get install -y --no-install-recommends \
        curl \
        apt-utils \
        openssl \
        ca-certificates

RUN apt-get update && apt-get install -y --no-install-recommends \
        git \
        g++ \
        make \
        libssl-dev \
        libgflags-dev \
        libprotobuf-dev \
        libprotoc-dev \
        protobuf-compiler \
        protobuf-compiler-grpc\
        libleveldb-dev \
        cmake \
        libgtest-dev \
        libsnappy-dev && \
        apt-get clean -y

RUN git clone https://github.com/apache/brpc.git

RUN cd brpc && sh config_brpc.sh --headers=/usr/include --libs=/usr/lib && \
    make -j "$(nproc)"

WORKDIR /app
COPY cpp_brpc_bench /app
COPY proto /app/proto

RUN cmake -B build && cmake --build build -j "$(nproc)"

ENTRYPOINT /app/build/main