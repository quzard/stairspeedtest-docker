FROM multiarch/qemu-user-static:x86_64-aarch64 as qemu
FROM arm64v8/ubuntu
COPY --from=qemu /usr/bin/qemu-aarch64-static /usr/bin
LABEL Auther="tindy2013" \
      Source="https://github.com/tindy2013/stairspeedtest-reborn" \
      Builder="We1eVen"
RUN apt-get update && \
    apt-get --no-install-recommends install curl wget ca-certificates -y && \
    ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo "Asia/Shanghai" > /etc/timezone && \
    apt-get install -y tzdata && \
    dpkg-reconfigure -f noninteractive tzdata

WORKDIR /speedtest
RUN wget https://github.com/tindy2013/stairspeedtest-reborn/releases/download/v0.7.0/stairspeedtest_reborn_arm64.tar.gz -O 070.tar.gz && \
    wget https://github.com/tindy2013/stairspeedtest-reborn/releases/download/v0.7.1/stairspeedtest_reborn_arm64.tar.gz -O 071.tar.gz && \
    mkdir -p 071 && mkdir -p 070 && \
    tar xf 070.tar.gz -C 070 && tar xf 071.tar.gz -C 071 && \
    wget https://raw.githubusercontent.com/We1eVen/stairspeedtest-docker/main/webgui.sh -O 070/stairspeedtest/webgui.sh && \
    mv 070/stairspeedtest/* . && mv -f 071/stairspeedtest/stairspeedtest . && \
    rm -rf 070 071 070.tar.gz 071.tar.gz && \
    mv tools/gui/gui.html tools/gui/index.html && \
    chmod -R +x *
ENV PORT=65430
ENV MAXFORKS=1
ENV THREAD=4
VOLUME ["/speedtest/results"]
EXPOSE 65430
ENTRYPOINT [ "./webgui.sh"]
