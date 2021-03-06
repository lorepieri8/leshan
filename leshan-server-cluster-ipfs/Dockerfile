#
# NOTE: THIS DOCKERFILE IS GENERATED VIA "update.sh"
#
# PLEASE DO NOT EDIT IT DIRECTLY.
#

FROM alpine:3.7

# A few reasons for installing distribution-provided OpenJDK:
#
#  1. Oracle.  Licensing prevents us from redistributing the official JDK.
#
#  2. Compiling OpenJDK also requires the JDK to be installed, and it gets
#     really hairy.
#
#     For some sample build times, see Debian's buildd logs:
#       https://buildd.debian.org/status/logs.php?pkg=openjdk-8

# Default to UTF-8 file.encoding
ENV LANG C.UTF-8

# add a simple script that can auto-detect the appropriate JAVA_HOME value
# based on whether the JDK or only the JRE is installed
RUN { \
		echo '#!/bin/sh'; \
		echo 'set -e'; \
		echo; \
		echo 'dirname "$(dirname "$(readlink -f "$(which javac || which java)")")"'; \
	} > /usr/local/bin/docker-java-home \
	&& chmod +x /usr/local/bin/docker-java-home
ENV JAVA_HOME /usr/lib/jvm/java-1.8-openjdk
ENV PATH $PATH:/usr/lib/jvm/java-1.8-openjdk/jre/bin:/usr/lib/jvm/java-1.8-openjdk/bin

ENV JAVA_VERSION 8u151
ENV JAVA_ALPINE_VERSION 8.171.11-r0

RUN set -x \
	&& apk add --no-cache \
		openjdk8="$JAVA_ALPINE_VERSION" \
	&& [ "$JAVA_HOME" = "$(docker-java-home)" ]

# If you're reading this and have any feedback on how this image could be
# improved, please open an issue or a pull request so we can discuss it!
#
#   https://github.com/docker-library/openjdk/issues


# Server web app
EXPOSE 8080

# IPFS ports
EXPOSE 4001
EXPOSE 5001

# Server COAP port
EXPOSE 5683

# Server COAP secure port
EXPOSE 5684

# Copy Leshan server application JAR (with libraries inside)
COPY target/leshan-server-cluster-ipfs-1.0.0-SNAPSHOT-jar-with-dependencies.jar /leshan-server-cluster-ipfs.jar

# Specify default command
CMD ["/usr/bin/java", "-jar", "/leshan-server-cluster-ipfs.jar", "-ipfs", "ipfs-ethereum", "-bn", "http://ganache-cli:8545", "-gl", "2000000", "-gp", "20000000000", "-pk", "0x76dda3572973659eabbd6c9279b66256838038da8189ee689e174e7acabfe3c5", "-sc", "0x3d18c830c5110e3d29c5dfff28719dee3cc3ed80"]
