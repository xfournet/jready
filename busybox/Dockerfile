ARG busybox_version=1.29.3
ARG debian_version=stretch-20180716

# Extract additionnal glibc files from the debian image
FROM debian:${debian_version} as debian

RUN mkdir -p /busybox/lib && \
  cp /lib/x86_64-linux-gnu/libdl-2.24.so /busybox/lib/libdl.so.2 && \
  cp /lib/x86_64-linux-gnu/librt.so.1 /busybox/lib/librt.so.1 && \
  cp /lib/x86_64-linux-gnu/libz.so.1.2.8 /busybox/lib/libz.so.1 && \
  cp /lib/x86_64-linux-gnu/libgcc_s.so.1 /busybox/lib/libgcc_s.so.1


# Add the additional glibc file in the busybox image
FROM busybox:${busybox_version}-glibc

ARG busybox_version
ARG debian_version
ARG vcs_ref
ARG image_version
ARG build_date

LABEL org.label-schema.schema-version="1.0" \
      org.label-schema.name="JRE ready image" \
      org.label-schema.url="https://github.com/xfournet/jready" \
      org.label-schema.vcs-url="https://github.com/xfournet/jready.git" \
      org.label-schema.vcs-ref="$vcs_ref" \
      org.label-schema.version="$image_version" \
      org.label-schema.build-date="$build_date"

COPY --from=debian /busybox/ /
