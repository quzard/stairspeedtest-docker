FROM multiarch/qemu-user-static:x86_64-aarch64 as qemu
FROM bxhdocker/alpine-gcc:edge as builder
WORKDIR /build
COPY --from=qemu /usr/bin/qemu-aarch64-static /usr/bin
RUN apk update && apk add bash git wget && \
    export ARCH=aarch64 && \
    git clone https://github.com/tindy2013/stairspeedtest-reborn.git && \
    cd stairspeedtest-reborn && \
    bash scripts/build.alpine.release.sh && \
    wget https://raw.githubusercontent.com/We1eVen/stairspeedtest-docker/main/webgui.sh -O stairspeedtest/webgui.sh

FROM arm64v8/alpine
WORKDIR /speedtest
#COPY --from=qemu /usr/bin/qemu-aarch64-static /usr/bin
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
