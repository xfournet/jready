# JRE ready Docker image
jready is a set of minimalist Docker images that are ready to execute a JRE.

More than a small size, the goal of minimal Docker image is to enhance security by reducing the attack surface.

Note than theses images don't include a JRE:
* you can freely choose the JRE vendor (eg Oracle or OpenJDK) and version (tested from 1.7 to 15)
* with JRE 9 and higher, you can freely build your own minimal JRE thanks to [jlink](https://docs.oracle.com/en/java/javase/15/docs/specs/man/jlink.html). See example below.

An example below shows how a JRE can be integrated on top of theses images.

## BusyBox based [(busybox/Dockerfile)](https://github.com/xfournet/jready/blob/main/busybox/Dockerfile)

Based on the offical [BusyBox glibc image](https://hub.docker.com/_/busybox)

[![](https://images.microbadger.com/badges/version/xfournet/jready:busybox-1.32.0.svg)](https://microbadger.com/images/xfournet/jready:busybox-1.32.0)
[![](https://images.microbadger.com/badges/image/xfournet/jready:busybox-1.32.0.svg)](https://microbadger.com/images/xfournet/jready:busybox-1.32.0)


## Alpine based [(alpine/Dockerfile)](https://github.com/xfournet/jready/blob/main/alpine/Dockerfile)

Based on the offical [Alpine image](https://hub.docker.com/_/alpine) and [Alpine GLIBC package](https://github.com/sgerrand/alpine-pkg-glibc)

[![](https://images.microbadger.com/badges/version/xfournet/jready:alpine-3.12.3.svg)](https://microbadger.com/images/xfournet/jready:alpine-3.12.3)
[![](https://images.microbadger.com/badges/image/xfournet/jready:alpine-3.12.3.svg)](https://microbadger.com/images/xfournet/jready:alpine-3.12.3)

# Example

This example is using AdoptOpenJDK 15. It's based from the `busybox` based image but `alpine` can be used instead with no other changes in the Dockerfile. 

**Dockerfile**

Create a minimal JRE image. If needed the `JLINK_MODULES` list (comma-separated) can be completed to fit other needs.

```Dockerfile
FROM xfournet/jready:busybox

ARG JDK_URL=https://github.com/AdoptOpenJDK/openjdk15-binaries/releases/download/jdk-15%2B36/OpenJDK15U-jdk_x64_linux_hotspot_15_36.tar.gz
ARG JLINK_OPTIONS="--vm=server --compress=2 --no-header-files --no-man-pages"
ARG JLINK_MODULES="java.base"
ARG JRE_DIR="/opt/jre"

RUN \
    # Download and extract JDK
    wget ${JDK_URL} -O - | tar xz -C /tmp && \
    # Build customized JRE
    /tmp/jdk-*/bin/jlink --output ${JRE_DIR} --add-modules ${JLINK_MODULES} ${JLINK_OPTIONS} && \
    # Remove JDK
    rm -r /tmp/jdk-* 
    
ENV PATH="${JRE_DIR}/bin:${PATH}"    
``` 

**Image build and test**

```bash
docker build -t myjre .
docker run --rm -ti myjre

# in container
java --version
```
