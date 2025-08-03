# Use OpenJDK 8 JRE (slim image)
FROM openjdk:8-jre-slim

# Metadata
LABEL maintainer="you@example.com"
LABEL description="Apache JMeter Docker image for CI/CD"

# Install dependencies
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        curl \
    && rm -rf /var/lib/apt/lists/*

# JMeter version (update as needed)
ARG JMETER_VERSION=5.6.3
ENV JMETER_HOME /opt/apache-jmeter-${JMETER_VERSION}
ENV PATH $JMETER_HOME/bin:$PATH

# Download and verify JMeter
RUN curl -L -o jmeter.tgz "https://archive.apache.org/dist/jmeter/binaries/apache-jmeter-${JMETER_VERSION}.tgz" \
    && echo "$(curl -L https://archive.apache.org/dist/jmeter/binaries/apache-jmeter-${JMETER_VERSION}.tgz.sha512)  jmeter.tgz" | sha512sum -c - \
    && mkdir -p /opt \
    && tar -xzf jmeter.tgz -C /opt \
    && rm jmeter.tgz

# Create non-root user for security
RUN addgroup --system jmeter \
    && adduser --system --ingroup jmeter jmeter \
    && chown -R jmeter:jmeter $JMETER_HOME

USER jmeter
WORKDIR $JMETER_HOME

# Volume for test plans and results
VOLUME ["/jmeter/testplan", "/jmeter/reports"]

# Default command: show JMeter version
ENTRYPOINT ["jmeter", "--version"]   
