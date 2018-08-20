ARG busybox_version=1.29.2
ARG debian_version=stretch-20180716

FROM debian:${debian_version} as debian

RUN mkdir -p /busybox/lib && \
  cp /lib/x86_64-linux-gnu/libdl-2.24.so /busybox/lib/libdl.so.2 && \
  cp /lib/x86_64-linux-gnu/librt.so.1 /busybox/lib/librt.so.1 && \
  cp /lib/x86_64-linux-gnu/libz.so.1.2.8 /busybox/lib/libz.so.1 && \
  cp /lib/x86_64-linux-gnu/libgcc_s.so.1 /busybox/lib/libgcc_s.so.1

FROM busybox:${busybox_version}-glibc
COPY --from=debian /busybox/ /
