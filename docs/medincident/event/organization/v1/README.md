# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [medincident/event/organization/v1/events.proto](#medincident_event_organization_v1_events-proto)
    - [Address](#medincident-event-organization-v1-Address)
    - [OrganizationAdminAssigned](#medincident-event-organization-v1-OrganizationAdminAssigned)
    - [OrganizationAdminDeputyAssigned](#medincident-event-organization-v1-OrganizationAdminDeputyAssigned)
    - [OrganizationAdminDeputyRemoved](#medincident-event-organization-v1-OrganizationAdminDeputyRemoved)
    - [OrganizationAdminRevoked](#medincident-event-organization-v1-OrganizationAdminRevoked)
    - [OrganizationCreated](#medincident-event-organization-v1-OrganizationCreated)
    - [OrganizationDetailsChanged](#medincident-event-organization-v1-OrganizationDetailsChanged)
    - [OrganizationDispatcherAssigned](#medincident-event-organization-v1-OrganizationDispatcherAssigned)
    - [OrganizationDispatcherDeputyAssigned](#medincident-event-organization-v1-OrganizationDispatcherDeputyAssigned)
    - [OrganizationDispatcherDeputyRemoved](#medincident-event-organization-v1-OrganizationDispatcherDeputyRemoved)
    - [OrganizationDispatcherRevoked](#medincident-event-organization-v1-OrganizationDispatcherRevoked)
    - [OrganizationHeadAssigned](#medincident-event-organization-v1-OrganizationHeadAssigned)
    - [OrganizationHeadDeputyAssigned](#medincident-event-organization-v1-OrganizationHeadDeputyAssigned)
    - [OrganizationHeadDeputyRemoved](#medincident-event-organization-v1-OrganizationHeadDeputyRemoved)
    - [OrganizationHeadRevoked](#medincident-event-organization-v1-OrganizationHeadRevoked)
    - [OrganizationLegalAddressChanged](#medincident-event-organization-v1-OrganizationLegalAddressChanged)
    - [Point](#medincident-event-organization-v1-Point)

- [Scalar Value Types](#scalar-value-types)



<a name="medincident_event_organization_v1_events-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## medincident/event/organization/v1/events.proto



<a name="medincident-event-organization-v1-Address"></a>

### Address



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| text | [string](#string) |  |  |
| point | [Point](#medincident-event-organization-v1-Point) | optional |  |






<a name="medincident-event-organization-v1-OrganizationAdminAssigned"></a>

### OrganizationAdminAssigned



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| employee_id | [string](#string) |  |  |






<a name="medincident-event-organization-v1-OrganizationAdminDeputyAssigned"></a>

### OrganizationAdminDeputyAssigned



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| employee_id | [string](#string) |  |  |
| deputy_employee_id | [string](#string) |  |  |






<a name="medincident-event-organization-v1-OrganizationAdminDeputyRemoved"></a>

### OrganizationAdminDeputyRemoved



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| employee_id | [string](#string) |  |  |






<a name="medincident-event-organization-v1-OrganizationAdminRevoked"></a>

### OrganizationAdminRevoked



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| employee_id | [string](#string) |  |  |






<a name="medincident-event-organization-v1-OrganizationCreated"></a>

### OrganizationCreated
OrganizationCreated — the aggregate entered this state on creation.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| name | [string](#string) |  |  |
| description | [string](#string) | optional |  |
| legal_address | [Address](#medincident-event-organization-v1-Address) |  |  |






<a name="medincident-event-organization-v1-OrganizationDetailsChanged"></a>

### OrganizationDetailsChanged
OrganizationDetailsChanged — name and description became these.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| name | [string](#string) |  |  |
| description | [string](#string) | optional |  |






<a name="medincident-event-organization-v1-OrganizationDispatcherAssigned"></a>

### OrganizationDispatcherAssigned



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| employee_id | [string](#string) |  |  |






<a name="medincident-event-organization-v1-OrganizationDispatcherDeputyAssigned"></a>

### OrganizationDispatcherDeputyAssigned



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| employee_id | [string](#string) |  |  |
| deputy_employee_id | [string](#string) |  |  |






<a name="medincident-event-organization-v1-OrganizationDispatcherDeputyRemoved"></a>

### OrganizationDispatcherDeputyRemoved



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| employee_id | [string](#string) |  |  |






<a name="medincident-event-organization-v1-OrganizationDispatcherRevoked"></a>

### OrganizationDispatcherRevoked



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| employee_id | [string](#string) |  |  |






<a name="medincident-event-organization-v1-OrganizationHeadAssigned"></a>

### OrganizationHeadAssigned



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| employee_id | [string](#string) |  |  |






<a name="medincident-event-organization-v1-OrganizationHeadDeputyAssigned"></a>

### OrganizationHeadDeputyAssigned



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| employee_id | [string](#string) |  |  |
| deputy_employee_id | [string](#string) |  |  |






<a name="medincident-event-organization-v1-OrganizationHeadDeputyRemoved"></a>

### OrganizationHeadDeputyRemoved



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| employee_id | [string](#string) |  |  |






<a name="medincident-event-organization-v1-OrganizationHeadRevoked"></a>

### OrganizationHeadRevoked



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| employee_id | [string](#string) |  |  |






<a name="medincident-event-organization-v1-OrganizationLegalAddressChanged"></a>

### OrganizationLegalAddressChanged
OrganizationLegalAddressChanged — legal address became this.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| legal_address | [Address](#medincident-event-organization-v1-Address) |  |  |






<a name="medincident-event-organization-v1-Point"></a>

### Point



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| longitude | [double](#double) |  |  |
| latitude | [double](#double) |  |  |















## Scalar Value Types

| .proto Type | Notes | C++ | Java | Python | Go | C# | PHP | Ruby |
| ----------- | ----- | --- | ---- | ------ | -- | -- | --- | ---- |
| <a name="double" /> double |  | double | double | float | float64 | double | float | Float |
| <a name="float" /> float |  | float | float | float | float32 | float | float | Float |
| <a name="int32" /> int32 | Uses variable-length encoding. Inefficient for encoding negative numbers – if your field is likely to have negative values, use sint32 instead. | int32 | int | int | int32 | int | integer | Bignum or Fixnum (as required) |
| <a name="int64" /> int64 | Uses variable-length encoding. Inefficient for encoding negative numbers – if your field is likely to have negative values, use sint64 instead. | int64 | long | int/long | int64 | long | integer/string | Bignum |
| <a name="uint32" /> uint32 | Uses variable-length encoding. | uint32 | int | int/long | uint32 | uint | integer | Bignum or Fixnum (as required) |
| <a name="uint64" /> uint64 | Uses variable-length encoding. | uint64 | long | int/long | uint64 | ulong | integer/string | Bignum or Fixnum (as required) |
| <a name="sint32" /> sint32 | Uses variable-length encoding. Signed int value. These more efficiently encode negative numbers than regular int32s. | int32 | int | int | int32 | int | integer | Bignum or Fixnum (as required) |
| <a name="sint64" /> sint64 | Uses variable-length encoding. Signed int value. These more efficiently encode negative numbers than regular int64s. | int64 | long | int/long | int64 | long | integer/string | Bignum |
| <a name="fixed32" /> fixed32 | Always four bytes. More efficient than uint32 if values are often greater than 2^28. | uint32 | int | int | uint32 | uint | integer | Bignum or Fixnum (as required) |
| <a name="fixed64" /> fixed64 | Always eight bytes. More efficient than uint64 if values are often greater than 2^56. | uint64 | long | int/long | uint64 | ulong | integer/string | Bignum |
| <a name="sfixed32" /> sfixed32 | Always four bytes. | int32 | int | int | int32 | int | integer | Bignum or Fixnum (as required) |
| <a name="sfixed64" /> sfixed64 | Always eight bytes. | int64 | long | int/long | int64 | long | integer/string | Bignum |
| <a name="bool" /> bool |  | bool | boolean | boolean | bool | bool | boolean | TrueClass/FalseClass |
| <a name="string" /> string | A string must always contain UTF-8 encoded or 7-bit ASCII text. | string | String | str/unicode | string | string | string | String (UTF-8) |
| <a name="bytes" /> bytes | May contain any arbitrary sequence of bytes. | string | ByteString | str | []byte | ByteString | string | String (ASCII-8BIT) |
