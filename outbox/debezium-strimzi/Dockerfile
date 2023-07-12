ARG DEBEZIUM_VERSION=2.4

FROM quay.io/debezium/connect:$DEBEZIUM_VERSION

ENV KAFKA_CONNECT_SHARE_JAVA_PATH=/kafka/libs \
    OPENTELEMETRY_VERSION=1.23.1 \
    OPENTELEMETRY_INSTRUMENTATION_VERSION=1.23.0 \
    OPENTELEMETRY_JAVAAGENT=1.23.0

ENV KAFKA_OPTS="-javaagent:/opentelemetry-javaagent-${OPENTELEMETRY_JAVAAGENT}.jar"

#TODO change to curl
COPY "debezium-interceptor-2.4.0-SNAPSHOT.jar" $KAFKA_CONNECT_SHARE_JAVA_PATH

WORKDIR /kafka/connect
#TODO change to curl
COPY "debezium-connector-postgres-2.4.0-SNAPSHOT-plugin.tar.gz" .

USER root

#TODO move to container image ?
RUN set -eux; \
    curl -sSL -o /opentelemetry-javaagent-${OPENTELEMETRY_JAVAAGENT}.jar https://github.com/open-telemetry/opentelemetry-java-instrumentation/releases/download/v${OPENTELEMETRY_JAVAAGENT}/opentelemetry-javaagent.jar; \
    curl -sSL -o ${KAFKA_CONNECT_SHARE_JAVA_PATH}/opentelemetry-api-${OPENTELEMETRY_VERSION}.jar https://repo1.maven.org/maven2/io/opentelemetry/opentelemetry-api/${OPENTELEMETRY_VERSION}/opentelemetry-api-${OPENTELEMETRY_VERSION}.jar; \
    curl -sSL -o ${KAFKA_CONNECT_SHARE_JAVA_PATH}/opentelemetry-context-${OPENTELEMETRY_VERSION}.jar https://repo1.maven.org/maven2/io/opentelemetry/opentelemetry-context/${OPENTELEMETRY_VERSION}/opentelemetry-context-${OPENTELEMETRY_VERSION}.jar; \
    curl -sSL -o ${KAFKA_CONNECT_SHARE_JAVA_PATH}/opentelemetry-semconv-${OPENTELEMETRY_VERSION}-alpha.jar https://repo1.maven.org/maven2/io/opentelemetry/opentelemetry-semconv/${OPENTELEMETRY_VERSION}-alpha/opentelemetry-semconv-${OPENTELEMETRY_VERSION}-alpha.jar; \
    curl -sSL -o ${KAFKA_CONNECT_SHARE_JAVA_PATH}/opentelemetry-instrumentation-api-${OPENTELEMETRY_INSTRUMENTATION_VERSION}.jar https://repo1.maven.org/maven2/io/opentelemetry/instrumentation/opentelemetry-instrumentation-api/${OPENTELEMETRY_INSTRUMENTATION_VERSION}/opentelemetry-instrumentation-api-${OPENTELEMETRY_INSTRUMENTATION_VERSION}.jar; \
    curl -sSL -o ${KAFKA_CONNECT_SHARE_JAVA_PATH}/opentelemetry-instrumentation-api-semconv-${OPENTELEMETRY_INSTRUMENTATION_VERSION}-alpha.jar https://repo1.maven.org/maven2/io/opentelemetry/instrumentation/opentelemetry-instrumentation-api-semconv/${OPENTELEMETRY_INSTRUMENTATION_VERSION}-alpha/opentelemetry-instrumentation-api-semconv-${OPENTELEMETRY_INSTRUMENTATION_VERSION}-alpha.jar; \
    curl -sSL -o ${KAFKA_CONNECT_SHARE_JAVA_PATH}/opentelemetry-kafka-clients-2.6-${OPENTELEMETRY_INSTRUMENTATION_VERSION}-alpha.jar https://repo1.maven.org/maven2/io/opentelemetry/instrumentation/opentelemetry-kafka-clients-2.6/${OPENTELEMETRY_INSTRUMENTATION_VERSION}-alpha/opentelemetry-kafka-clients-2.6-${OPENTELEMETRY_INSTRUMENTATION_VERSION}-alpha.jar; \
    curl -sSL -o ${KAFKA_CONNECT_SHARE_JAVA_PATH}/opentelemetry-kafka-clients-common-${OPENTELEMETRY_INSTRUMENTATION_VERSION}-alpha.jar https://repo1.maven.org/maven2/io/opentelemetry/instrumentation/opentelemetry-kafka-clients-common/${OPENTELEMETRY_INSTRUMENTATION_VERSION}-alpha/opentelemetry-kafka-clients-common-${OPENTELEMETRY_INSTRUMENTATION_VERSION}-alpha.jar; \
    chmod 666 ${KAFKA_CONNECT_SHARE_JAVA_PATH}/opentelemetry-api-${OPENTELEMETRY_VERSION}.jar \
    ${KAFKA_CONNECT_SHARE_JAVA_PATH}/opentelemetry-context-${OPENTELEMETRY_VERSION}.jar  \
    ${KAFKA_CONNECT_SHARE_JAVA_PATH}/opentelemetry-semconv-${OPENTELEMETRY_VERSION}-alpha.jar  \
    ${KAFKA_CONNECT_SHARE_JAVA_PATH}/opentelemetry-instrumentation-api-${OPENTELEMETRY_INSTRUMENTATION_VERSION}.jar  \
    ${KAFKA_CONNECT_SHARE_JAVA_PATH}/opentelemetry-instrumentation-api-semconv-${OPENTELEMETRY_INSTRUMENTATION_VERSION}-alpha.jar  \
    ${KAFKA_CONNECT_SHARE_JAVA_PATH}/opentelemetry-kafka-clients-2.6-${OPENTELEMETRY_INSTRUMENTATION_VERSION}-alpha.jar  \
    ${KAFKA_CONNECT_SHARE_JAVA_PATH}/opentelemetry-kafka-clients-common-${OPENTELEMETRY_INSTRUMENTATION_VERSION}-alpha.jar  \
    ${KAFKA_CONNECT_SHARE_JAVA_PATH}/debezium-interceptor-2.4.0-SNAPSHOT.jar;

#TODO change to curl
RUN set -eux; \
    rm -r debezium-connector-{db2,jdbc,mongodb,mysql,oracle,postgres,spanner,sqlserver,vitess}; \
    tar xzvf debezium-connector-postgres-2.4.0-SNAPSHOT-plugin.tar.gz

USER 1001