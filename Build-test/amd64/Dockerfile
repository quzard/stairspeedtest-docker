FROM multiarch/alpine:amd64-latest-stable as builder
WORKDIR /build
RUN apk update && apk add bash git && \
    git clone https://github.com/tindy2013/stairspeedtest-reborn.git && \
    cd stairspeedtest-reborn && \
    bash scripts/build.alpine.release.sh

FROM multiarch/alpine:amd64-latest-stable
WORKDIR /speedtest
COPY --from=builder /build/stairspeedtest-reborn/stairspeedtest .
RUN apk update && apk add tzdata wget && \
    cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && \
    wget https://raw.githubusercontent.com/We1eVen/stairspeedtest-docker/main/webgui.sh -O webgui.sh && \
    chmod -R +x *
ENV PORT=65430
ENV MAXFORKS=1
ENV THREAD=4
VOLUME ["/speedtest/results"]
EXPOSE 65430
ENTRYPOINT [ "./webgui.sh"]
