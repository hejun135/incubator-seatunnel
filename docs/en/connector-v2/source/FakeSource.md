# FakeSource

> FakeSource connector

## Description

The FakeSource is a virtual data source, which randomly generates the number of rows according to the data structure of the user-defined schema,
just for some test cases such as type conversion or connector new feature testing

## Key features

- [x] [batch](../../concept/connector-v2-features.md)
- [x] [stream](../../concept/connector-v2-features.md)
- [ ] [exactly-once](../../concept/connector-v2-features.md)
- [x] [column projection](../../concept/connector-v2-features.md)
- [ ] [parallelism](../../concept/connector-v2-features.md)
- [ ] [support user-defined split](../../concept/connector-v2-features.md)

## Options

| name                | type   | required | default value |
|---------------------|--------|----------|---------------|
| schema              | config | yes      | -             |
| rows                | config | no       | -             |
| row.num             | int    | no       | 5             |
| split.num           | int    | no       | 1             |
| split.read-interval | long   | no       | 1             |
| map.size            | int    | no       | 5             |
| array.size          | int    | no       | 5             |
| bytes.length        | int    | no       | 5             |
| string.length       | int    | no       | 5             |
| common-options      |        | no       | -             |

### schema [config]

#### fields [Config]

The schema of fake data that you want to generate

#### Examples

```hocon
  schema = {
    fields {
      c_map = "map<string, array<int>>"
      c_array = "array<int>"
      c_string = string
      c_boolean = boolean
      c_tinyint = tinyint
      c_smallint = smallint
      c_int = int
      c_bigint = bigint
      c_float = float
      c_double = double
      c_decimal = "decimal(30, 8)"
      c_null = "null"
      c_bytes = bytes
      c_date = date
      c_timestamp = timestamp
      c_row = {
        c_map = "map<string, map<string, string>>"
        c_array = "array<int>"
        c_string = string
        c_boolean = boolean
        c_tinyint = tinyint
        c_smallint = smallint
        c_int = int
        c_bigint = bigint
        c_float = float
        c_double = double
        c_decimal = "decimal(30, 8)"
        c_null = "null"
        c_bytes = bytes
        c_date = date
        c_timestamp = timestamp
      }
    }
  }
```

### rows

The row list of fake data output per degree of parallelism

example

```hocon
  rows = [
    {
      kind = INSERT
      fields = [1, "A", 100]
    },
    {
      kind = UPDATE_BEFORE
      fields = [1, "A", 100]
    },
    {
      kind = UPDATE_AFTER
      fields = [1, "A_1", 100]
    },
    {
      kind = DELETE
      fields = [1, "A_1", 100]
    }
  ]
```

### row.num

The total number of data generated per degree of parallelism

### split.num

the number of splits generated by the enumerator for each degree of parallelism

### split.read-interval

The interval(mills) between two split reads in a reader

### map.size

The size of `map` type that connector generated

### array.size

The size of `array` type that connector generated

### bytes.length

The length of `bytes` type that connector generated

### string.length

The length of `string` type that connector generated

### common options

Source plugin common parameters, please refer to [Source Common Options](common-options.md) for details

## Example

Auto generate data rows

```hocon
FakeSource {
  row.num = 10
  map.size = 10
  array.size = 10
  bytes.length = 10
  string.length = 10
  schema = {
    fields {
      c_map = "map<string, array<int>>"
      c_array = "array<int>"
      c_string = string
      c_boolean = boolean
      c_tinyint = tinyint
      c_smallint = smallint
      c_int = int
      c_bigint = bigint
      c_float = float
      c_double = double
      c_decimal = "decimal(30, 8)"
      c_null = "null"
      c_bytes = bytes
      c_date = date
      c_timestamp = timestamp
      c_row = {
        c_map = "map<string, map<string, string>>"
        c_array = "array<int>"
        c_string = string
        c_boolean = boolean
        c_tinyint = tinyint
        c_smallint = smallint
        c_int = int
        c_bigint = bigint
        c_float = float
        c_double = double
        c_decimal = "decimal(30, 8)"
        c_null = "null"
        c_bytes = bytes
        c_date = date
        c_timestamp = timestamp
      }
    }
  }
}
```

Using fake data rows

```hocon
FakeSource {
  schema = {
    fields {
      pk_id = bigint
      name = string
      score = int
    }
  }
  rows = [
    {
      kind = INSERT
      fields = [1, "A", 100]
    },
    {
      kind = INSERT
      fields = [2, "B", 100]
    },
    {
      kind = INSERT
      fields = [3, "C", 100]
    },
    {
      kind = UPDATE_BEFORE
      fields = [1, "A", 100]
    },
    {
      kind = UPDATE_AFTER
      fields = [1, "A_1", 100]
    },
    {
      kind = DELETE
      fields = [2, "B", 100]
    }
  ]
}
```

## Changelog

### 2.2.0-beta 2022-09-26

- Add FakeSource Source Connector

### 2.3.0-beta 2022-10-20

- [Improve] Supports direct definition of data values(row) ([2839](https://github.com/apache/incubator-seatunnel/pull/2839))
- [Improve] Improve fake source connector: ([2944](https://github.com/apache/incubator-seatunnel/pull/2944))
  - Support user-defined map size
  - Support user-defined array size
  - Support user-defined string length
  - Support user-defined bytes length
- [Improve] Support multiple splits for fake source connector ([2974](https://github.com/apache/incubator-seatunnel/pull/2974))
- [Improve] Supports setting the number of splits per parallelism and the reading interval between two splits ([3098](https://github.com/apache/incubator-seatunnel/pull/3098))

### next version

- [Feature] Support config fake data rows [3865](https://github.com/apache/incubator-seatunnel/pull/3865)