# Data Types

DataFusion uses Arrow, and thus the Arrow type system, for query
execution. The SQL types from
[sqlparser-rs](https://github.com/sqlparser-rs/sqlparser-rs/blob/main/src/ast/data_type.rs#L27)
are mapped to [Arrow data types](https://docs.rs/arrow/latest/arrow/datatypes/enum.DataType.html) according to the following table.
This mapping occurs when defining the schema in a `CREATE EXTERNAL TABLE` command or when performing a SQL `CAST` operation.

You can see the corresponding Arrow type for any SQL expression using
the `arrow_typeof` function. For example:

```sql
select arrow_typeof(interval '1 month');
+---------------------------------------------------------------------+
| arrow_typeof(IntervalMonthDayNano("79228162514264337593543950336")) |
+---------------------------------------------------------------------+
| Interval(MonthDayNano)                                              |
+---------------------------------------------------------------------+
```

You can cast a SQL expression to a specific Arrow type using the `arrow_cast` function
For example, to cast the output of `now()` to a `Timestamp` with second precision:

```sql
select arrow_cast(now(), 'Timestamp(Second, None)');
+---------------------+
| now()               |
+---------------------+
| 2023-03-03T17:19:21 |
+---------------------+
```

## Character Types

| SQL DataType | Arrow DataType |
| ------------ | -------------- |
| `CHAR`       | `Utf8`         |
| `VARCHAR`    | `Utf8`         |
| `TEXT`       | `Utf8`         |
| `STRING`     | `Utf8`         |

## Numeric Types

| SQL DataType                         | Arrow DataType                 | Notes                                                                                                 |
| ------------------------------------ | :----------------------------- | ----------------------------------------------------------------------------------------------------- |
| `TINYINT`                            | `Int8`                         |                                                                                                       |
| `SMALLINT`                           | `Int16`                        |                                                                                                       |
| `INT` or `INTEGER`                   | `Int32`                        |                                                                                                       |
| `BIGINT`                             | `Int64`                        |                                                                                                       |
| `TINYINT UNSIGNED`                   | `UInt8`                        |                                                                                                       |
| `SMALLINT UNSIGNED`                  | `UInt16`                       |                                                                                                       |
| `INT UNSIGNED` or `INTEGER UNSIGNED` | `UInt32`                       |                                                                                                       |
| `BIGINT UNSIGNED`                    | `UInt64`                       |                                                                                                       |
| `FLOAT`                              | `Float32`                      |                                                                                                       |
| `REAL`                               | `Float32`                      |                                                                                                       |
| `DOUBLE`                             | `Float64`                      |                                                                                                       |
| `DECIMAL(precision, scale)`          | `Decimal128(precision, scale)` | Decimal support is currently experimental ([#3523](https://github.com/apache/datafusion/issues/3523)) |

## Date/Time Types

| SQL DataType | Arrow DataType                   |
| ------------ | :------------------------------- |
| `DATE`       | `Date32`                         |
| `TIME`       | `Time64(Nanosecond)`             |
| `TIMESTAMP`  | `Timestamp(Nanosecond, None)`    |
| `INTERVAL`   | `Interval(IntervalMonthDayNano)` |

## Boolean Types

| SQL DataType | Arrow DataType |
| ------------ | :------------- |
| `BOOLEAN`    | `Boolean`      |

## Binary Types

| SQL DataType | Arrow DataType |
| ------------ | :------------- |
| `BYTEA`      | `Binary`       |

You can create binary literals using a hex string literal such as
`X'1234'` to create a `Binary` value of two bytes, `0x12` and `0x34`.

## Unsupported SQL Types

| SQL Data Type | Arrow DataType      |
| ------------- | :------------------ |
| `UUID`        | _Not yet supported_ |
| `BLOB`        | _Not yet supported_ |
| `CLOB`        | _Not yet supported_ |
| `BINARY`      | _Not yet supported_ |
| `VARBINARY`   | _Not yet supported_ |
| `REGCLASS`    | _Not yet supported_ |
| `NVARCHAR`    | _Not yet supported_ |
| `CUSTOM`      | _Not yet supported_ |
| `ARRAY`       | _Not yet supported_ |
| `ENUM`        | _Not yet supported_ |
| `SET`         | _Not yet supported_ |
| `DATETIME`    | _Not yet supported_ |

## Supported Arrow Types

The following types are supported by the `arrow_typeof` function:

| Arrow Type                                                  |
| ----------------------------------------------------------- |
| `Null`                                                      |
| `Boolean`                                                   |
| `Int8`                                                      |
| `Int16`                                                     |
| `Int32`                                                     |
| `Int64`                                                     |
| `UInt8`                                                     |
| `UInt16`                                                    |
| `UInt32`                                                    |
| `UInt64`                                                    |
| `Float16`                                                   |
| `Float32`                                                   |
| `Float64`                                                   |
| `Utf8`                                                      |
| `LargeUtf8`                                                 |
| `Binary`                                                    |
| `Timestamp(Second, None)`                                   |
| `Timestamp(Millisecond, None)`                              |
| `Timestamp(Microsecond, None)`                              |
| `Timestamp(Nanosecond, None)`                               |
| `Time32`                                                    |
| `Time64`                                                    |
| `Duration(Second)`                                          |
| `Duration(Millisecond)`                                     |
| `Duration(Microsecond)`                                     |
| `Duration(Nanosecond)`                                      |
| `Interval(YearMonth)`                                       |
| `Interval(DayTime)`                                         |
| `Interval(MonthDayNano)`                                    |
| `FixedSizeBinary(<len>)` (e.g. `FixedSizeBinary(16)`)       |
| `Decimal128(<precision>, <scale>)` e.g. `Decimal128(3, 10)` |
| `Decimal256(<precision>, <scale>)` e.g. `Decimal256(3, 10)` |