FROM multiarch/alpine:amd64-latest-stable as builder
WORKDIR /build
RUN apk add bash git wget && \
    export ARCH=x86_64 && \
    git clone https://github.com/tindy2013/stairspeedtest-reborn.git && \
    cd stairspeedtest-reborn && \
    wget https://raw.githubusercontent.com/quzard/stairspeedtest-docker/master/pref.ini -O base/pref.ini && \
    bash scripts/build.alpine.release.sh && \
    wget https://raw.githubusercontent.com/quzard/stairspeedtest-docker/master/webgui.sh -O stairspeedtest/webgui.sh

FROM alpine:latest
WORKDIR /speedtest
COPY --from=builder /build/stairspeedtest-reborn/stairspeedtest .
RUN apk add bash tzdata && \
    cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && \
    echo Asia/Shanghai > /etc/timezone && \
    chmod -R +x *
ENV PORT=65430
ENV MAXFORKS=1
ENV THREAD=4
VOLUME ["/speedtest/results"]
EXPOSE 65430
ENTRYPOINT ["./webgui.sh"]
