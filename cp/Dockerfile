FROM solsson/kafka-jre@sha256:61196845eafb0970cf94598fd723a66407be31cd8bab83e3a29b4e602683280d

ENV CP_VERSION=3.3.0

WORKDIR /usr/local

RUN set -ex; \
  WORKDIR=$PWD; \
  mkdir -p $WORKDIR/share/java; \
  \
  export DEBIAN_FRONTEND=noninteractive; \
  runDeps=''; \
  buildDeps='curl ca-certificates'; \
  apt-get update && apt-get install -y $runDeps $buildDeps --no-install-recommends; \
  \
  MAVEN_VERSION=3.5.0 PATH=$PATH:/opt/maven/bin; \
  mkdir -p /opt/maven; \
  curl -SLs https://archive.apache.org/dist/maven/maven-3/$MAVEN_VERSION/binaries/apache-maven-$MAVEN_VERSION-bin.tar.gz | tar -xzf - --strip-components=1 -C /opt/maven; \
  mvn --version; \
  \
  mkdir -p /opt/src/common; cd /opt/src/common; \
  curl -SLs https://github.com/confluentinc/common/archive/v$CP_VERSION.tar.gz | tar -xzf - --strip-components=1 -C ./; \
  mvn install; \
  \
  mkdir -p /opt/src/rest-utils; cd /opt/src/rest-utils; \
  curl -SLs https://github.com/confluentinc/rest-utils/archive/v$CP_VERSION.tar.gz | tar -xzf - --strip-components=1 -C ./; \
  mvn install; \
  \
  mkdir -p /opt/src/schema-registry; cd /opt/src/schema-registry; \
  curl -SLs https://github.com/confluentinc/schema-registry/archive/v$CP_VERSION.tar.gz | tar -xzf - --strip-components=1 -C ./; \
  mvn install; \
  \
  mkdir -p /opt/src/kafka-rest; cd /opt/src/kafka-rest; \
  curl -SLs https://github.com/confluentinc/kafka-rest/archive/v$CP_VERSION.tar.gz | tar -xzf - --strip-components=1 -C ./; \
  mvn install; \
  \
  cd $WORKDIR; \
  \
  mv /opt/src/common/package/target/common-package-$CP_VERSION-package/share/java/confluent-common ./share/java/; \
  mv /opt/src/rest-utils/package/target/rest-utils-package-$CP_VERSION-package/share/java/rest-utils ./share/java/; \
  \
  mv /opt/src/schema-registry/package-schema-registry/target/kafka-schema-registry-package-$CP_VERSION-package/bin/* ./bin/; \
  mv /opt/src/schema-registry/package-schema-registry/target/kafka-schema-registry-package-$CP_VERSION-package/share/java/* ./share/java/; \
  mv /opt/src/schema-registry/package-schema-registry/target/kafka-schema-registry-package-$CP_VERSION-package/etc/* /etc/; \
  \
  mv /opt/src/kafka-rest/target/kafka-rest-$CP_VERSION-package/bin/* ./bin/; \
  mv /opt/src/kafka-rest/target/kafka-rest-$CP_VERSION-package/share/java/* ./share/java/; \
  mv /opt/src/kafka-rest/target/kafka-rest-$CP_VERSION-package/etc/* /etc; \
  \
  rm -Rf /opt/src /opt/maven /root/.m2; \
  \
  apt-get purge -y --auto-remove $buildDeps; \
  rm -rf /var/lib/apt/lists/*; \
  rm -rf /var/log/dpkg.log /var/log/alternatives.log /var/log/apt

COPY docker-help.sh /usr/local/bin/docker-help
ENTRYPOINT ["docker-help"]
