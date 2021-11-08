# Schema Registry Overview

Confluent Schema Registry provides a serving layer for your metadata. It provides a RESTful interface for storing and retrieving your [Avro®](https://avro.apache.org/docs/current/spec.html), [JSON Schema](https://json-schema.org/), and [Protobuf](https://developers.google.com/protocol-buffers/) [schemas](https://docs.confluent.io/platform/current/schema-registry/schema_registry_onprem_tutorial.html#schema-registry-tutorial-definition). It stores a versioned history of all schemas based on a specified [subject name strategy](https://docs.confluent.io/platform/current/schema-registry/serdes-develop/index.html#sr-schemas-subject-name-strategy), provides multiple [compatibility settings](https://docs.confluent.io/platform/current/schema-registry/avro.html#schema-evolution-and-compatibility) and allows evolution of schemas according to the configured compatibility settings and expanded support for these schema types. It provides serializers that plug into Apache Kafka® clients that handle schema storage and retrieval for Kafka messages that are sent in any of the supported formats.

Schema Registry lives outside of and separately from your Kafka brokers. Your producers and consumers still talk to Kafka to publish and read data (messages) to topics. Concurrently, they can also talk to Schema Registry to send and retrieve schemas that describe the data models for the messages.

![../_images/schema-registry-and-kafka.png](https://docs.confluent.io/platform/current/_images/schema-registry-and-kafka.png)

Confluent Schema Registry for storing and retrieving schemas

Schema Registry is a distributed storage layer for schemas which uses Kafka as its underlying storage mechanism. Some key design decisions:

- Assigns globally unique ID to each registered schema. Allocated IDs are guaranteed to be monotonically increasing and unique, but not necessarily consecutive.
- Kafka provides the durable backend, and functions as a write-ahead changelog for the state of Schema Registry and the schemas it contains.
- Schema Registry is designed to be distributed, with single-primary architecture, and ZooKeeper/Kafka coordinates primary election (based on the configuration).

See also

To see a working example of Schema Registry, check out [Confluent Platform demo](https://docs.confluent.io/platform/current/tutorials/cp-demo/docs/index.html#cp-demo). The demo shows you how to deploy a Kafka streaming ETL, including Schema Registry, using ksqlDB for stream processing.



## Schemas, Subjects, and Topics

First, a quick review of terms and how they fit in the context of Schema Registry: what is a Kafka topic versus a schema versus a subject.

A Kafka topic contains messages, and each message is a key-value pair. Either the message key or the message value, or both, can be serialized as Avro, JSON, or Protobuf. A schema defines the structure of the data format. The Kafka topic name can be independent of the schema name. Schema Registry defines a scope in which schemas can evolve, and that scope is the subject. The name of the subject depends on the configured [subject name strategy](https://docs.confluent.io/platform/current/schema-registry/serdes-develop/index.html#sr-schemas-subject-name-strategy), which by default is set to derive subject name from topic name.

Starting with Confluent Platform 5.5.0, you can modify the subject name strategy on a per-topic basis. See [Change the subject naming strategy for a topic](https://docs.confluent.io/platform/current/schema-registry/schema-validation.html#sr-per-topic-subject-name-strategy) to learn more.

The [Schema Registry Tutorials](https://docs.confluent.io/platform/current/schema-registry/schema_registry_tutorial.html#schema-registry-tutorial) shows an example of a [schema definition](https://docs.confluent.io/platform/current/schema-registry/schema_registry_onprem_tutorial.html#schema-registry-tutorial-definition).

Starting with Confluent Platform 7.0.0, Schema Linking is available in preview, as described in [Schema Linking on Confluent Platform (Preview)](https://docs.confluent.io/platform/current/schema-registry/schema-linking-cp.html#schema-linking-cp-overview).

Starting with Confluent Platform 5.2.0, you can use Confluent Replicator to [migrate schemas](https://docs.confluent.io/platform/current/schema-registry/index.html#schemas-migrate-overview) from one Schema Registry to another, and automatically rename subjects on the target registry.

## Kafka Serializers and Deserializers Background

When sending data over the network or storing it in a file, you need a way to encode the data into bytes. The area of data serialization has a long history, but has evolved quite a bit over the last few years. People started with programming language specific serialization such as Java serialization, which makes consuming the data in other languages inconvenient, then moved to language agnostic formats such as pure JSON, but without a strictly defined schema format.

Not having a strictly defined format has two significant drawbacks:

1. **Data consumers may not understand data producers:** The lack of structure makes consuming data in these formats more challenging because fields can be arbitrarily added or removed, and data can even be corrupted. This drawback becomes more severe the more applications or teams across an organization begin consuming a data feed: if an upstream team can make arbitrary changes to the data format at their discretion, then it becomes very difficult to ensure that all downstream consumers will (continue to) be able to interpret the data. What’s missing is a “contract” (cf. schema below) for data between the producers and the consumers, similar to the contract of an API.
2. **Overhead and verbosity:** They are verbose because field names and type information have to be explicitly represented in the serialized format, despite the fact that are identical across all messages.

A few cross-language serialization libraries have emerged that require the data structure to be formally defined by schemas. These libraries include [Avro](https://avro.apache.org/), [Thrift](http://thrift.apache.org/), [Protocol Buffers](https://developers.google.com/protocol-buffers/), and [JSON Schema](https://json-schema.org/) . The advantage of having a schema is that it clearly specifies the structure, the type and the meaning (through documentation) of the data. With a schema, data can also be encoded more efficiently. Avro was the default supported format for Confluent Platform.

For example, an Avro schema defines the data structure in a JSON format. The following Avro schema specifies a user record with two fields: `name` and `favorite_number` of type `string` and `int`, respectively.

```
{"namespace": "example.avro",
 "type": "record",
 "name": "user",
 "fields": [
     {"name": "name", "type": "string"},
     {"name": "favorite_number",  "type": "int"}
 ]
}
```



You can then use this Avro schema, for example, to serialize a Java object (POJO) into bytes, and deserialize these bytes back into the Java object.

Avro not only requires a schema during data serialization, but also during data deserialization. Because the schema is provided at decoding time, metadata such as the field names don’t have to be explicitly encoded in the data. This makes the binary encoding of Avro data very compact.



## Avro, JSON, and Protobuf Supported Formats and Extensibility

Avro was the original choice for the default supported schema format in Confluent Platform, with Kafka serializers and deserializers provided for the Avro format.

Confluent Platform supports for [Protocol Buffers](https://developers.google.com/protocol-buffers/) and [JSON Schema](https://json-schema.org/) along with [Avro](https://avro.apache.org/), the original default format for Confluent Platform. Support for these new serialization formats is not limited to Schema Registry, but provided throughout Confluent Platform. Additionally, Schema Registry is extensible to support adding custom schema formats as schema plugins.

New Kafka serializers and deserializers are available for Protobuf and JSON Schema, along with Avro. The serializers can automatically register schemas when serializing a Protobuf message or a JSON-serializable object. The Protobuf serializer can recursively register all imported schemas, .

The serializers and deserializers are available in multiple languages, including Java, .NET and Python.

Schema Registry supports multiple formats at the same time. For example, you can have Avro schemas in one subject and Protobuf schemas in another. Furthermore, both Protobuf and JSON Schema have their own compatibility rules, so you can have your Protobuf schemas evolve in a backward or forward compatible manner, just as with [Avro](https://avro.apache.org/).

Schema Registry in Confluent Platform also supports for [schema references](https://docs.confluent.io/platform/current/schema-registry/serdes-develop/index.html#referenced-schemas) in Protobuf by modeling the import statement.

To learn more, see [Formats, Serializers, and Deserializers](https://docs.confluent.io/platform/current/schema-registry/serdes-develop/index.html#serializer-and-formatter).

## Schema ID Allocation

Schema ID allocation always happens in the primary node and Schema IDs are always monotonically increasing.

In Kafka primary election, the Schema ID is always based off the last ID that was written to Kafka store. During a primary re-election, batch allocation happens only after the new primary has caught up with all the records in the store `<kafkastore.topic>`.



## Kafka Backend

Kafka is used as Schema Registry storage backend. The special Kafka topic `<kafkastore.topic>` (default `_schemas`), with a single partition, is used as a highly available write ahead log. All schemas, subject/version and ID metadata, and compatibility settings are appended as messages to this log. A Schema Registry instance therefore both produces and consumes messages under the `_schemas` topic. It produces messages to the log when, for example, new schemas are registered under a subject, or when updates to compatibility settings are registered. Schema Registry consumes from the `_schemas` log in a background thread, and updates its local caches on consumption of each new `_schemas` message to reflect the newly added schema or compatibility setting. Updating local state from the Kafka log in this manner ensures durability, ordering, and easy recoverability.

Tip

The Schema Registry topic is compacted and therefore the latest value of every key is retained forever, regardless of the Kafka retention policy. You can validate this with `kafka-configs`:

```
kafka-configs --bootstrap-server localhost:9092 --entity-type topics --entity-name _schemas --describe
```



Your output should resemble:

```
Configs for topic '_schemas' are cleanup.policy=compact
```





## Single Primary Architecture

Schema Registry is designed to work as a distributed service using single primary architecture. In this configuration, at most one Schema Registry instance is the primary at any given moment (ignoring pathological ‘zombie primaries’). Only the primary is capable of publishing writes to the underlying Kafka log, but all nodes are capable of directly serving read requests. Secondary nodes serve registration requests indirectly by simply forwarding them to the current primary, and returning the response supplied by the primary. Starting with Confluent Platform 4.0, primary election is accomplished with the Kafka group protocol. (ZooKeeper based primary election was removed in Confluent Platform 7.0.0. )

Note

Please make sure not to mix up the election modes amongst the nodes in same cluster. This will lead to multiple primaries and issues with your operations.

### Kafka Coordinator Primary Election

![../_images/schema-registry-design-kafka.png](https://docs.confluent.io/platform/current/_images/schema-registry-design-kafka.png)

Kafka based Schema Registry

Kafka based primary election is chosen when [kafkastore.connection.url](https://docs.confluent.io/platform/current/schema-registry/installation/config.html#kafkastore-connection-url) is not configured and has the Kafka bootstrap brokers `<kafkastore.bootstrap.servers>` specified. The Kafka group protocol, chooses one amongst the primary eligible nodes `leader.eligibility=true` as the primary. Kafka based primary election should be used in all cases. (ZooKeeper based leader election was removed in Confluent Platform 7.0.0. See [Migration from ZooKeeper primary election to Kafka primary election](https://docs.confluent.io/platform/current/schema-registry/installation/deployment.html#schemaregistry-zk-migration).)

Schema Registry is also designed for multi-colocated configuration. See [Multi-Datacenter Setup](https://docs.confluent.io/platform/current/schema-registry/multidc.html#schemaregistry-mirroring) for more details.

### ZooKeeper Primary Election

Important

ZooKeeper leader election was removed in Confluent Platform 7.0.0. Kafka leader election should be used instead. See [Migration from ZooKeeper primary election to Kafka primary election](https://docs.confluent.io/platform/current/schema-registry/installation/deployment.html#schemaregistry-zk-migration) for full details.



### High Availability for Single Primary Setup

Many services in Confluent Platform are effectively stateless (they store state in Kafka and load it on demand at start-up) and can redirect requests automatically. You can treat these services as you would deploying any other stateless application and get high availability features effectively for free by deploying multiple instances. Each instance loads all of the Schema Registry state so any node can serve a READ, and all nodes know how to forward requests to the primary for WRITEs.

A recommended approach is to put the instances behind a single virtual IP or round robin DNS and define multiple URLs for the Schema Registry client in `schema.registry.url`, thereby using the entire cluster of Schema Registry instances and providing a method for failover.

This also makes it easy to handle changes to the set of servers without having to reconfigure and restart all of your applications. The same strategy applies to REST proxy or Kafka Connect.

A simple setup with just a few nodes means Schema Registry can fail over easily with a simple multi-node deployment and single primary election protocol.

Alternatively, you can use a single URL for `schema.registry.url` and still use the entire cluster of Schema Registry instances. However, this configuration does not support failover to different Schema Registry instances in a dynamic DNS or virtual IP setup because only one `schema.registry.url` is surfaced to schema registry clients.



## Migrate Schemas (Confluent Cloud and self-managed)

Starting with Confluent Platform 7.0.0, Schema Linking is available in preview, as described in [Schema Linking on Confluent Platform (Preview)](https://docs.confluent.io/platform/current/schema-registry/schema-linking-cp.html#schema-linking-cp-overview).

Starting with Confluent Platform 5.2.0, you can use [Replicator](https://docs.confluent.io/platform/current/multi-dc-deployments/replicator/index.html#replicator-detail) to migrate

schemas from a self-managed cluster to a target cluster which is either self-managed or in [Confluent Cloud](https://docs.confluent.io/cloud/current/index.html).

- For a concept overview and quick start tutorial on migrating schemas from self-managed clusters to Confluent Cloud, see [Migrate Schemas to Confluent Cloud](https://docs.confluent.io/platform/current/schema-registry/installation/migrate.html#migrate-self-managed-schemas-to-cloud).
- For a demo of migrating schemas from one self-managed cluster to another, see [Migrate Schemas](https://docs.confluent.io/platform/current/schema-registry/installation/migrate.html#schemaregistry-migrate) and [Replicator Schema Translation Example](https://docs.confluent.io/platform/current/tutorials/examples/replicator-schema-translation/docs/index.html#quickstart-demos-replicator-schema-translation).



## License

Schema Registry is licensed under the [Confluent Community License](https://www.confluent.io/confluent-community-license).

A Confluent Enterprise license is required for the [Schema Registry Security Plugin](https://docs.confluent.io/platform/current/confluent-security-plugins/schema-registry/introduction.html#confluentsecurityplugins-schema-registry-security-plugin) and for broker-side [Schema Validation on Confluent Server](https://docs.confluent.io/platform/current/schema-registry/schema-validation.html#schema-validation).

You can use the plugin and Schema Validation under a 30-day trial period without a license key, and thereafter under an [Enterprise (Subscription) License](https://docs.confluent.io/platform/current/installation/license.html#cp-enterprise-subs-license) as part of Confluent Platform.

To learn more more about the security plugin, see [License for Schema Registry Security Plugin](https://docs.confluent.io/platform/current/schema-registry/installation/config.html#schema-registry-license-config) and [Install and Configure the Schema Registry Security Plugin](https://docs.confluent.io/platform/current/confluent-security-plugins/schema-registry/install.html#confluentsecurityplugins-schema-registry-security-quickstart).

Tip

For complete license information for Confluent Platform, see [Confluent Platform Licenses](https://docs.confluent.io/platform/current/installation/license.html#cp-license-overview).

## Suggested Reading

- [Schema Registry Tutorials](https://docs.confluent.io/platform/current/schema-registry/schema_registry_tutorial.html#schema-registry-tutorial) (Confluent Cloud and on-premises)
- [Quick Start for Schema Management on Confluent Cloud](https://docs.confluent.io/cloud/current/get-started/schema-registry.html)
- [Run an automated Confluent Cloud quickstart with Avro, Protobuf, and JSON formats](https://github.com/confluentinc/examples/tree/7.0.1-post/cp-quickstart/README.md)
- [Use Control Center to manage schemas for on-premises deployments](https://docs.confluent.io/platform/current/control-center/topics/schema.html#topicschema)
- [Installing and Configuring Schema Registry](https://docs.confluent.io/platform/current/schema-registry/installation/index.html#schema-registry-quickstart)
- [Running Schema Registry in Production](https://docs.confluent.io/platform/current/schema-registry/installation/deployment.html#schema-registry-prod)
- [Schema Validation on Confluent Server](https://docs.confluent.io/platform/current/schema-registry/schema-validation.html#schema-validation)
- [Schema Registry Configuration Options](https://docs.confluent.io/platform/current/schema-registry/installation/config.html#schemaregistry-config)
- For Developers: [Formats, Serializers, and Deserializers](https://docs.confluent.io/platform/current/schema-registry/serdes-develop/index.html#serializer-and-formatter) and [Schema Registry Development Overview](https://docs.confluent.io/platform/current/schema-registry/develop/index.html#schemaregistry-dev-guide).

### Blog Posts

- [Schemas, Contracts, and Compatibility](https://www.confluent.io/blog/schemas-contracts-compatibility)
- [17 Ways to Mess Up Self-Managed Schema Registry](https://www.confluent.io/blog/17-ways-to-mess-up-self-managed-schema-registry)
- [Yes, Virginia, You Really Do Need a Schema Registry](https://www.confluent.io/blog/schema-registry-kafka-stream-processing-yes-virginia-you-really-need-one/)
- [How I Learned to Stop Worrying and Love the Schema](https://www.confluent.io/blog/how-i-learned-to-stop-worrying-and-love-the-schema-part-1/)

© Copyright 2021 , Confluent, Inc. [Privacy Policy](https://www.confluent.io/confluent-privacy-statement/) | [Terms & Conditions](https://www.confluent.io/terms-of-use/). Apache, Apache Kafka, Ka