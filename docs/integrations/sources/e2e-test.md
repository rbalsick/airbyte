# End-to-End Testing Source

## Overview

This is a mock source for testing the Airbyte pipeline. It can generate arbitrary data streams.

## Mode

### Continuous Feed

**This is the only mode available on Airbyte Cloud.**

This mode allows users to specify a single-stream or multi-stream catalog with arbitrary schema. The schema should be compliant with Json schema [draft-07](https://json-schema.org/draft-07/json-schema-release-notes.html).

The single-stream catalog config exists just for convenient, since in many testing cases, one stream is enough. If only one stream is specified in the multi-stream catalog config, it is equivalent to a single-stream catalog config.

Here is its configuration:

| Mock Catalog Type | Parameters | Type | Required | Default | Notes |
| --- | --- | --- | --- | --- | --- |
| Single-stream | stream name | string | yes | | Name of the stream in the catalog. |
| | stream schema | json | yes | | Json schema of the stream in the catalog. It must be a valid Json schema. |
| Multi-stream | streams and schemas | json | yes | | A Json object specifying multiple data streams and their schemas. Each key in this object is one stream name. Each value is the schema for that stream. |
| Both | max records | integer | yes | 100 | The number of record messages to emit from this connector. Min 1. Max 100 billion. |
| | random seed | integer | no | current time millis | The seed is used in random Json object generation. Min 0. Max 1 million. |
| | message interval | integer | no | 0 | The time interval between messages in millisecond. Min 0 ms. Max 60000 ms (1 minute). |

### Legacy Infinite Feed

This is a legacy mode used in Airbyte integration tests. It has a simple catalog with one `data` stream that has the following schema:

```json
{
  "type": "object",
  "properties":
    {
      "column1": { "type": "string" }
    }
}
```

The value of `column1` will be an increasing number starting from `1`.

This mode can generate infinite number of records, which can be dangerous. That's why it is excluded from the Cloud variant of this connector. Usually this mode should not be used.

There are two configurable parameters:

| Parameters | Type | Required | Default | Notes |
| --- | --- | --- | --- | --- |
| max records | integer | no | `null` | Number of message records to emit. When it is left empty, the connector will generate infinite number of messages. |
| message interval | integer | no | `null` | Time interval between messages in millisecond. |

### Exception after N

This is a legacy mode used in Airbyte integration tests. It throws an `IllegalStateException` after certain number of messages. The number of messages to emit before exception is the only parameter for this mode.

This mode is also excluded from the Cloud variant of this connector.

## Changelog

### OSS

| Version | Date | Pull request | Notes |
| --- | --- | --- | --- |
| 1.0.0 | 2021-01-23 | [\#9720](https://github.com/airbytehq/airbyte/pull/9720) | Add new continuous feed mode that supports arbitrary catalog specification. |
| 0.1.1 | 2021-12-16 | [\#8217](https://github.com/airbytehq/airbyte/pull/8217) | Fix sleep time in infinite feed mode. |
| 0.1.0 | 2021-07-23 | [\#3290](https://github.com/airbytehq/airbyte/pull/3290) [\#4939](https://github.com/airbytehq/airbyte/pull/4939) | Initial release. |

### Cloud

| Version | Date | Pull request | Notes |
| --- | --- | --- | --- |
| 1.0.0 | 2021-01-23 | [\#9720](https://github.com/airbytehq/airbyte/pull/9720) | Add new continuous feed mode that supports arbitrary catalog specification. Initial release to cloud. |
