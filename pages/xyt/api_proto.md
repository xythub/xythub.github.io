---
title: Protocol Buffers API
keywords: documentation, API documentation, API, single API, tick data, xythub, xyt hub, xyt-hub, big xyt, big-xyt
summary: "Data API Overview."
sidebar: xyt_sidebar
permalink: api_proto.html
folder: xyt
---


# <a class="anchor" name="api_data.proto"></a> api_data.proto
Defines contract file for data retrieval layer API.

API version: `2.0.0`


## <a class="anchor" name="api_data.proto.types"></a> Types
### <a class="anchor" name="api_data.proto.DataRequest"></a> Message: DataRequest

Generic data request.


| Field | Type | Id  | Options | Description |
| ----- | ---- | --- | ------- | ----------- |
| header | [RequestHeader](#types.proto.RequestHeader) | 1 |  | Request header. |
| parameters | map < string,  [Value](#types.proto.Value) >  | 2 |  | Request parameters. |

### <a class="anchor" name="api_data.proto.DataResponse"></a> Message: DataResponse

Generic data response.


| Field | Type | Id  | Options | Description |
| ----- | ---- | --- | ------- | ----------- |
| header | [ResponseHeader](#types.proto.ResponseHeader) | 1 |  | Response header. |
| data | [Table](#types.proto.Table) | 2 |  | Retrieved data. |
| meta | map < string,  [Value](#types.proto.Value) >  | 3 |  | Additional information. |

# <a class="anchor" name="types.proto"></a> types.proto
Defines common data types used by xyt APIs.

API version: `2.0.0`


## <a class="anchor" name="types.proto.types"></a> Types
### <a class="anchor" name="types.proto.RequestHeader"></a> Message: RequestHeader

Request header allows client to pass additional information with the request.


| Field | Type | Id  | Options | Description |
| ----- | ---- | --- | ------- | ----------- |
| reference_id | [UUID](#types.proto.UUID) | 1 |  | Cross reference identifier assigned to the request by the client. |

### <a class="anchor" name="types.proto.ResponseHeader"></a> Message: ResponseHeader

Response header allow service to pass additional information with the response.


| Field | Type | Id  | Options | Description |
| ----- | ---- | --- | ------- | ----------- |
| request_id | [UUID](#types.proto.UUID) | 1 |  | Identifier assigned to the request by the service. |
| reference_id | [UUID](#types.proto.UUID) | 2 |  | Cross reference identifier assigned to the request by the client. Copied from [RequestHeader](#types.proto.RequestHeader). |
| warning_message | string | 4 |  | Warning message. |

### <a class="anchor" name="types.proto.UUID"></a> Message: UUID

Represents UUID in human-readable format.


| Field | Type | Id  | Options | Description |
| ----- | ---- | --- | ------- | ----------- |
| value | string | 1 |  | UUID representation. |

### <a class="anchor" name="types.proto.Date"></a> Message: Date

Represents date.


| Field | Type | Id  | Options | Description |
| ----- | ---- | --- | ------- | ----------- |
| days | int32 | 10 |  | Number of days since 1970.01.01. |

### <a class="anchor" name="types.proto.Time"></a> Message: Time

Represents time during a day as milliseconds.


| Field | Type | Id  | Options | Description |
| ----- | ---- | --- | ------- | ----------- |
| milliseconds | int32 | 1 |  | Number of milliseconds since midnight. |
| nanoseconds | int32 | 2 |  | Nanoseconds part. |

### <a class="anchor" name="types.proto.Timestamp"></a> Message: Timestamp

Represents time and date as nanoseconds.


| Field | Type | Id  | Options | Description |
| ----- | ---- | --- | ------- | ----------- |
| nanoseconds | int64 | 1 |  | Number of nanoseconds from midnight 1970.01.01. |

### <a class="anchor" name="types.proto.Value"></a> Message: Value

Represents a dynamically typed value which can be either:
an integer, a double, a string, a boolean, a time, a date or a list of values.


| Field | Type | Id  | Options | Description |
| ----- | ---- | --- | ------- | ----------- |
| type | one of |  |  |  |
| type.string_value | string | 1 |  |  |
| type.integer_value | int64 | 2 |  |  |
| type.double_value | double | 3 |  |  |
| type.bool_value | bool | 4 |  |  |
| type.date_value | [Date](#types.proto.Date) | 5 | deprecated = true |  |
| type.time_value | [Time](#types.proto.Time) | 6 | deprecated = true |  |
| type.days_value | int32 | 8 |  | Number of days since 1970.01.01. |
| type.milliseconds_value | int32 | 9 |  | Number of milliseconds since midnight. |
| type.nanoseconds_value | int64 | 10 |  | Number of nanoseconds since midnight. |
| type.timestamp_value | int64 | 11 |  | Number of nanoseconds since midnight 1970.01.01. |
| type.object | [ValueObject](#types.proto.ValueObject) | 14 |  |  |
| type.list | [ValueList](#types.proto.ValueList) | 15 |  |  |

### <a class="anchor" name="types.proto.MixedList"></a> Message: MixedList

Represents a list of dynamically typed values which can be either:
an integer, a double, a string, a boolean, a time, a date.


| Field | Type | Id  | Options | Description |
| ----- | ---- | --- | ------- | ----------- |
| values | repeated  [Value](#types.proto.Value) | 1 |  |  |

### <a class="anchor" name="types.proto.ValueList"></a> Message: ValueList

Represents a dynamically typed list.


| Field | Type | Id  | Options | Description |
| ----- | ---- | --- | ------- | ----------- |
| list_type | one of |  |  |  |
| list_type.string_list | [StringList](#types.proto.StringList) | 1 |  |  |
| list_type.integer_list | [IntegerList](#types.proto.IntegerList) | 2 |  |  |
| list_type.double_list | [DoubleList](#types.proto.DoubleList) | 3 |  |  |
| list_type.bool_list | [BoolList](#types.proto.BoolList) | 4 |  |  |
| list_type.date_list | [DateList](#types.proto.DateList) | 5 | deprecated = true |  |
| list_type.time_list | [TimeList](#types.proto.TimeList) | 6 | deprecated = true |  |
| list_type.days_list | [DaysList](#types.proto.DaysList) | 8 |  |  |
| list_type.milliseconds_list | [MillisecondsList](#types.proto.MillisecondsList) | 9 |  |  |
| list_type.nanoseconds_list | [NanosecondsList](#types.proto.NanosecondsList) | 10 |  |  |
| list_type.timestamps_list | [TimestampsList](#types.proto.TimestampsList) | 11 |  |  |
| list_type.mixed_list | [MixedList](#types.proto.MixedList) | 15 |  |  |

### <a class="anchor" name="types.proto.ValueObject"></a> Message: ValueObject

Represents an object with named fields.


| Field | Type | Id  | Options | Description |
| ----- | ---- | --- | ------- | ----------- |
| fields | map < string,  [Value](#types.proto.Value) >  | 1 |  |  |

### <a class="anchor" name="types.proto.Table"></a> Message: Table

Represents a data table as a list of named columns.


| Field | Type | Id  | Options | Description |
| ----- | ---- | --- | ------- | ----------- |
| columns | map < string,  [ValueList](#types.proto.ValueList) >  | 1 |  |  |

### <a class="anchor" name="types.proto.StringList"></a> Message: StringList

Represents a list of strings.


| Field | Type | Id  | Options | Description |
| ----- | ---- | --- | ------- | ----------- |
| values | repeated  string | 1 |  |  |

### <a class="anchor" name="types.proto.IntegerList"></a> Message: IntegerList

Represents a list of integers.


| Field | Type | Id  | Options | Description |
| ----- | ---- | --- | ------- | ----------- |
| values | repeated  int64 | 1 |  | Null values are represented as Long.MIN_VALUE (-2^63) |

### <a class="anchor" name="types.proto.DoubleList"></a> Message: DoubleList

Represents a list of numeric values.


| Field | Type | Id  | Options | Description |
| ----- | ---- | --- | ------- | ----------- |
| values | repeated  double | 1 |  | Null values are represented as NaN |

### <a class="anchor" name="types.proto.BoolList"></a> Message: BoolList

Represents a list of boolean values.


| Field | Type | Id  | Options | Description |
| ----- | ---- | --- | ------- | ----------- |
| values | repeated  bool | 1 |  |  |

### <a class="anchor" name="types.proto.DateList"></a> Message: DateList

Represents a list of date values.


| Field | Type | Id  | Options | Description |
| ----- | ---- | --- | ------- | ----------- |
| values | repeated  [Date](#types.proto.Date) | 1 |  |  |

### <a class="anchor" name="types.proto.TimeList"></a> Message: TimeList

Represents a list of time values.


| Field | Type | Id  | Options | Description |
| ----- | ---- | --- | ------- | ----------- |
| values | repeated  [Time](#types.proto.Time) | 1 |  |  |

### <a class="anchor" name="types.proto.DaysList"></a> Message: DaysList

Represents a list of days (since 1970.01.01) values.


| Field | Type | Id  | Options | Description |
| ----- | ---- | --- | ------- | ----------- |
| values | repeated  int32 | 1 |  |  |

### <a class="anchor" name="types.proto.TimestampsList"></a> Message: TimestampsList

Represents a list of nanoseconds (since 1970.01.01) values.


| Field | Type | Id  | Options | Description |
| ----- | ---- | --- | ------- | ----------- |
| values | repeated  int64 | 1 |  |  |

### <a class="anchor" name="types.proto.MillisecondsList"></a> Message: MillisecondsList

Represents a list of milliseconds (since midnight) values.


| Field | Type | Id  | Options | Description |
| ----- | ---- | --- | ------- | ----------- |
| values | repeated  int32 | 1 |  |  |

### <a class="anchor" name="types.proto.NanosecondsList"></a> Message: NanosecondsList

Represents a list of nanoseconds (since midnight) values.


| Field | Type | Id  | Options | Description |
| ----- | ---- | --- | ------- | ----------- |
| values | repeated  int64 | 1 |  |  |

### <a class="anchor" name="types.proto.ProductType"></a> Enum: ProductType


| Value | Id  | Description |
| ----- | --- | ----------- |
| TRADES | 0 | Trades only. |
| QUOTES | 1 | Trades and quotes. |
| MARKET_BY_PRICE | 2 | Level 2 market data: market by price. |
| MARKET_BY_ORDER | 3 | Level 3 market data: market by order. |
| OTHER_PRODUCT | 15 | Reserved. |

### <a class="anchor" name="types.proto.ItemType"></a> Enum: ItemType


| Value | Id  | Description |
| ----- | --- | ----------- |
| INDIVIDUAL_ITEM | 0 | Single traded instrument. |
| OPTION_CHAIN | 1 | Options series. |
| FUTURE_CHAIN | 2 | Future series. |
| ENTIRE_EXCHANGE | 3 | Entire exchange. |
| OTHER_ITEM | 15 | Reserved. |

### <a class="anchor" name="types.proto.ChainType"></a> Enum: ChainType


| Value | Id  | Description |
| ----- | --- | ----------- |
| OPTIONS | 0 | Options. |
| FUTURES | 1 | Futures. |
| FUTURE_OPTIONS | 2 | Future options. |


{% include links.html %}
