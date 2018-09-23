# JRE ready Docker image
jready is a set of minimalist Docker images that are ready to execute a JRE.

Theses images don't include a JRE:
* you can freely to choose the JRE vendor (Oracle or OpenJDK) and version
* with JRE 9 and higher, you can freely build your own minimal JRE thanks to [jlink](https://docs.oracle.com/javase/10/tools/jlink.htm)

## BusyBox based

Based on the offical [busybox glibc image](https://hub.docker.com/_/busybox)

[![](https://images.microbadger.com/badges/version/xfournet/jready:busybox-1.29.2.svg)](https://microbadger.com/images/xfournet/jready:busybox-1.29.2)
[![](https://images.microbadger.com/badges/image/xfournet/jready:busybox-1.29.2.svg)](https://microbadger.com/images/xfournet/jready:busybox-1.29.2)


## Alpine based

Based on the offical [alpine image](https://hub.docker.com/_/alpine) and [Alpine GLIBC package](https://github.com/sgerrand/alpine-pkg-glibc)

[![](https://images.microbadger.com/badges/version/xfournet/jready:alpine-3.8.svg)](https://microbadger.com/images/xfournet/jready:alpine-3.8)
[![](https://images.microbadger.com/badges/image/xfournet/jready:alpine-3.8.svg)](https://microbadger.com/images/xfournet/jready:alpine-3.8)
