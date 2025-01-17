# Testcontainers for ZIO


| CI                              | Release                                                                                                                                                                                                                                                                       |
| ------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ![Build Status][Badge-CircelCI] | [![Release Artifacts][Badge-SonatypeReleases-Kafka]][Link-SonatypeReleases-Kafka] [![Release Artifacts][Badge-SonatypeReleases-MySQL]][Link-SonatypeReleases-MySQL] [![Release Artifacts][Badge-SonatypeReleases-DbMigrationAspect]][Link-SonatypeReleases-DbMigrationAspect] |


[Badge-CircelCI]: https://circleci.com/gh/scottweaver/testcontainers-for-zio.svg?style=shield "CircleCI Badge"

[Link-Github]: https://github.com/scottweaver/testcontainers-for-zio "Github Repo Link"

[Link-SonatypeReleases-Kafka]: https://oss.sonatype.org/content/repositories/releases/io/github/scottweaver/zio-testcontainers-kafka_2.13/0.2.0/  "Sonatype Releases link"
[Badge-SonatypeReleases-Kafka]: https://img.shields.io/maven-central/v/io.github.scottweaver/zio-testcontainers-kafka_2.13/0.2.0?label=maven-central%20%20kafka "Sonatype Releases badge"

[Link-SonatypeReleases-MySQL]: https://oss.sonatype.org/content/repositories/releases/io/github/scottweaver/zio-testcontainers-mysql_2.13/0.2.0/  "Sonatype Releases link"
[Badge-SonatypeReleases-MySQL]: https://img.shields.io/maven-central/v/io.github.scottweaver/zio-testcontainers-mysql_2.13/0.2.0?label=maven-central%20%20mysql "Sonatype Releases badge"

[Link-SonatypeReleases-DbMigrationAspect]: https://oss.sonatype.org/content/repositories/releases/io/github/scottweaver/zio-db-migration-aspect_2.13/0.2.0/  "Sonatype Releases link"
[Badge-SonatypeReleases-DbMigrationAspect]: https://img.shields.io/maven-central/v/io.github.scottweaver/zio-db-migration-aspect_2.13/0.2.0?label=maven-central%20%20db-migration-aspect "Sonatype Releases badge"

Provides idiomatic, easy-to-use ZLayers for [Testcontainers-scala](https://github.com/testcontainers/testcontainers-scala).

## Testcontainers Best-Practices

- Make sure your test configuration has the following settings `Test / fork := true`. Without this  the Docker container created by the test will NOT be cleaned up until you exit SBT/the JVM process.  This could quickly run your machine out of resources as you will end up with a ton of orphaned containers running.
- Use `provideLayerShared({container}.live)` on your suite so that each test case isn't spinning up and down the container.

## MySQL

Provides a managed ZLayer that starts and stops a `com.dimafeng.testcontainers.MySQLTestContainer` as well as also provding a managed `java.sql.Connection`.

```scala
libraryDependencies += "io.github.scottweaver" %% "zio-testcontainers-mysql" % "0.2.0"
```

See test cases for example usage.

## Kafka

Provides a managed ZLayer that starts and stops a `com.dimafeng.testcontainers.KafkaContainer`.

You also have easy access to:
- `zio.kafka.consumer.ConsumerSettings` via `ZKafkaContainer.defaultConsumerSettings`.
- `zio.kafka.consumer.ProducerSettings` via `ZKafkaContainer.defaultProducerSettings`.

You can use these to create a `zio.kafka.consumer.Consumer` and/or `zio.kafka.producer.Producer` that can be used to interact with the Kafka container instance.


```scala
libraryDependencies += "io.github.scottweaver" %% "zio-testcontainers-kafka" % "0.2.0"
```

See test cases for example usage.

## Database Migrations Aspect

Not really a test container, useful none the less.  

The `io.github.scottweaver.zio.aspect.DatabaseMigrationsAspect` provides a [ZIO TestAspect](https://javadoc.io/doc/dev.zio/zio-test_2.13/1.0.12/zio/test/TestAspect.html) for running database migrations via [Flyway](https://flywaydb.org/).  It seemlessly integrates with the `ZMySQLContainer` by using the `io.github.scottweaver.zio.models.JdbcInfo` provided by `ZMySQLContainer.live` to run your migrations.

If you are not using `ZMySQLContainer` you can just manually provide an appropriate `JdbcInfo` as a `ZLayer` to your tests that are using the `DbMigrationAspect`.

```scala
libraryDependencies += "io.github.scottweaver" %% "db-migration-aspect" % "0.2.0"
```

## References

- [Working with shared dependencies in ZIO Test](https://hmemcpy.com/2021/11/working-with-shared-dependencies-in-zio-test/)
