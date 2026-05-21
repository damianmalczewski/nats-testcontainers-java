# Testcontainers NATS Module

A [Testcontainers](https://www.testcontainers.org/) module for [NATS](https://nats.io/) messaging system.

## Features

- Easy NATS container setup for integration testing
- JetStream support
- Authentication support (username/password)
- Convenient access to connection URLs and ports
- Debug and protocol tracing options

## Installation

[![Maven Central Version](https://img.shields.io/maven-central/v/io.github.amadeusitgroup.testcontainers/nats)](https://central.sonatype.com/artifact/io.github.amadeusitgroup.testcontainers/nats)

Add the dependency to your `pom.xml`:

```xml
<dependency>
    <groupId>io.github.amadeusitgroup.testcontainers</groupId>
    <artifactId>nats</artifactId>
    <version>{version}</version>
    <scope>test</scope>
</dependency>
```

See latest version at [Releases](https://github.com/AmadeusITGroup/nats-testcontainers-java/releases) page.

## Usage

### Basic Usage

```java
try (NatsContainer nats = new NatsContainer("nats:2.12.1")) {
    nats.start();
    
    Options options = new Options.Builder()
        .server(nats.getConnectionUrl())
        .build();
    
    try (Connection nc = Nats.connect(options)) {
        // Use the NATS connection
    }
}
```

### With JetStream

```java
try (NatsContainer nats = new NatsContainer("nats:2.12.1").withJetStream()) {
    nats.start();
    
    Options options = new Options.Builder()
        .server(nats.getConnectionUrl())
        .build();
    
    try (Connection nc = Nats.connect(options)) {
        JetStream js = nc.jetStream();
        // Use JetStream
    }
}
```

### With Authentication

```java
try (NatsContainer nats = new NatsContainer("nats:2.12.1")
        .withAuth("username", "password")) {
    nats.start();
    
    Options options = new Options.Builder()
        .server(nats.getConnectionUrl())
        .userInfo("username", "password")
        .build();
    
    try (Connection nc = Nats.connect(options)) {
        // Use authenticated connection
    }
}
```

## API Reference

### Mapped Ports

| Method | Container Port | Description |
|--------|----------------|-------------|
| `getClientPort()` | 4222 | Returns the mapped host port for NATS client connections |
| `getRoutingPort()` | 6222 | Returns the mapped host port for cluster/route connections |
| `getHttpMonitoringPort()` | 8222 | Returns the mapped host port for HTTP monitoring |

### URLs

| Method | Description |
|--------|-------------|
| `getConnectionUrl()` | Returns `nats://host:port` for client connections |
| `getHttpMonitoringUrl()` | Returns `http://host:port` for monitoring endpoint |

### Configuration Methods

| Method | Description |
|--------|-------------|
| `withJetStream()` | Enables JetStream persistence |
| `withAuth(username, password)` | Configures authentication |
| `withDebug()` | Enables debug logging |
| `withProtocolTracing()` | Enables protocol tracing |

### Configuration Accessors

| Method | Returns | Description |
|--------|---------|-------------|
| `isJetStreamEnabled()` | `boolean` | Whether JetStream was enabled via `withJetStream()` |
| `isDebugEnabled()` | `boolean` | Whether debug logging was enabled via `withDebug()` |
| `isProtocolTracingEnabled()` | `boolean` | Whether protocol tracing was enabled via `withProtocolTracing()` |
| `getUsername()` | `String` | Username configured via `withAuth()`, or `null` |
| `getPassword()` | `String` | Password configured via `withAuth()`, or `null` |

## Building

```bash
mvn clean install
```

## Running Tests

```bash
mvn test
```

**Note:** Tests require Docker / Podman to be available.

## Contributing

Please read [CONTRIBUTING.md](CONTRIBUTING.md) for details on our [Code of Conduct](CODE_OF_CONDUCT.md), and the process for submitting pull requests to us.

## License

Apache License 2.0
