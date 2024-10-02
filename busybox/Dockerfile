ARG busybox_image=busybox:1.37.0-glibc
ARG debian_image=debian:buster-20210408-slim

# Extract additionnal glibc files from the debian image
FROM ${debian_image} as debian

RUN mkdir -p /busybox/lib && \
  cp -p /lib/x86_64-linux-gnu/libdl.so.2 /busybox/lib/libdl.so.2 && \
  cp -p /lib/x86_64-linux-gnu/librt.so.1 /busybox/lib/librt.so.1 && \
  cp -p /lib/x86_64-linux-gnu/libz.so.1 /busybox/lib/libz.so.1 && \
  cp -p /lib/x86_64-linux-gnu/libgcc_s.so.1 /busybox/lib/libgcc_s.so.1


# Add the additional glibc file in the busybox image
FROM ${busybox_image} as base

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


# Smoke test
FROM base as test

ARG JDK_URL=https://github.com/AdoptOpenJDK/openjdk15-binaries/releases/download/jdk-15%2B36/OpenJDK15U-jdk_x64_linux_hotspot_15_36.tar.gz
ARG JLINK_OPTIONS="--vm=server --compress=2 --no-header-files --no-man-pages"
ARG JLINK_MODULES="java.base,jdk.compiler"
ARG JRE_DIR="/opt/jre"

ENV PATH="${JRE_DIR}/bin:${PATH}"

RUN \
    # Download and extract JDK
    wget ${JDK_URL} -O - | tar xz -C /tmp && \
    # Build customized JRE
    /tmp/jdk-*/bin/jlink --output ${JRE_DIR} --add-modules ${JLINK_MODULES} ${JLINK_OPTIONS} && \
    # Remove JDK
    rm -r /tmp/jdk-* && \
    echo -e "\
class HelloWorld {\n\
    public static void main(String[] args) {\n\
        System.out.println(\"Hello World!\");\n\
    }\n\
}\n" > HelloWorld.java && \
    java HelloWorld.java


# Default target is 'base' image
FROM base
