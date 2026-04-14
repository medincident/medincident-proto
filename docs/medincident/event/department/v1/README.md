# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [medincident/event/department/v1/events.proto](#medincident_event_department_v1_events-proto)
    - [DepartmentCreated](#medincident-event-department-v1-DepartmentCreated)
    - [DepartmentDetailsChanged](#medincident-event-department-v1-DepartmentDetailsChanged)
    - [DepartmentResponsibleAssigned](#medincident-event-department-v1-DepartmentResponsibleAssigned)
    - [DepartmentResponsibleDeputyAssigned](#medincident-event-department-v1-DepartmentResponsibleDeputyAssigned)
    - [DepartmentResponsibleDeputyRemoved](#medincident-event-department-v1-DepartmentResponsibleDeputyRemoved)
    - [DepartmentResponsibleRevoked](#medincident-event-department-v1-DepartmentResponsibleRevoked)

- [Scalar Value Types](#scalar-value-types)



<a name="medincident_event_department_v1_events-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## medincident/event/department/v1/events.proto



<a name="medincident-event-department-v1-DepartmentCreated"></a>

### DepartmentCreated



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| clinic_id | [string](#string) |  |  |
| name | [string](#string) |  |  |
| description | [string](#string) | optional |  |






<a name="medincident-event-department-v1-DepartmentDetailsChanged"></a>

### DepartmentDetailsChanged



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| name | [string](#string) |  |  |
| description | [string](#string) | optional |  |






<a name="medincident-event-department-v1-DepartmentResponsibleAssigned"></a>

### DepartmentResponsibleAssigned
DepartmentResponsibleAssigned — an employee became the &#34;responsible&#34;
of this department. Payload carries the holder; aggregate_id in the
envelope is department_id.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| employee_id | [string](#string) |  |  |






<a name="medincident-event-department-v1-DepartmentResponsibleDeputyAssigned"></a>

### DepartmentResponsibleDeputyAssigned
DepartmentResponsibleDeputyAssigned — a deputy was attached to the
role held by employee_id. The deputy must currently work in the
same department.


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| employee_id | [string](#string) |  |  |
| deputy_employee_id | [string](#string) |  |  |






<a name="medincident-event-department-v1-DepartmentResponsibleDeputyRemoved"></a>

### DepartmentResponsibleDeputyRemoved
DepartmentResponsibleDeputyRemoved — the deputy slot was cleared
(explicit removal, cascade from role revocation, or cascade from
deputy employee&#39;s termination or department transfer).


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| employee_id | [string](#string) |  |  |






<a name="medincident-event-department-v1-DepartmentResponsibleRevoked"></a>

### DepartmentResponsibleRevoked
DepartmentResponsibleRevoked — role revoked (either explicitly or
as a cascade from transfer/termination).


| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| employee_id | [string](#string) |  |  |















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
