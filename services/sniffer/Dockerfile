FROM debian:stable

ENV DEBIAN_FRONTEND=noninteractive \
    PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        bash \
        ca-certificates \
        iproute2 \
        jq \
        net-tools \
        procps \
        python3 \
        python3-pip \
        suricata \
        tcpdump \
        tshark \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app

COPY scripts/ /app/scripts/
COPY python/ /app/python/

RUN find /opt/aviatre/scripts -type f -name '*.sh' -exec chmod +x {} +

CMD ["bash"]