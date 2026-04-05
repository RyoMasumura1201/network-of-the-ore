FROM ubuntu:22.04

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        iproute2 \
        iputils-ping \
        iptables \
    && rm -rf /var/lib/apt/lists/*

CMD ["/bin/bash"]
