# Kafka broker

FROM java:openjdk-8-jre

ENV SCALA_VERSION 2.11
ENV KAFKA_VERSION 1.0.2
ENV KAFKA_HOME /opt/kafka_"$SCALA_VERSION"-"$KAFKA_VERSION"

# Install Kafka
RUN wget -q http://apache.mirrors.spacedump.net/kafka/"$KAFKA_VERSION"/kafka_"$SCALA_VERSION"-"$KAFKA_VERSION".tgz -O /tmp/kafka_"$SCALA_VERSION"-"$KAFKA_VERSION".tgz && \
    tar xfz /tmp/kafka_"$SCALA_VERSION"-"$KAFKA_VERSION".tgz -C /opt && \
    rm /tmp/kafka_"$SCALA_VERSION"-"$KAFKA_VERSION".tgz

# kafka port
EXPOSE 9092

CMD $KAFKA_HOME/bin/kafka-server-start.sh $KAFKA_HOME/config/server.properties --override zookeeper.connect=tre-zookeeper