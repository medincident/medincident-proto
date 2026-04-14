# Protocol Documentation
<a name="top"></a>

## Table of Contents

- [medincident/service/membership/v1/membership.proto](#medincident_service_membership_v1_membership-proto)
    - [AssignClinicHeadDeputyRequest](#medincident-service-membership-v1-AssignClinicHeadDeputyRequest)
    - [AssignClinicHeadDeputyResponse](#medincident-service-membership-v1-AssignClinicHeadDeputyResponse)
    - [AssignClinicHeadRequest](#medincident-service-membership-v1-AssignClinicHeadRequest)
    - [AssignClinicHeadResponse](#medincident-service-membership-v1-AssignClinicHeadResponse)
    - [AssignDepartmentResponsibleDeputyRequest](#medincident-service-membership-v1-AssignDepartmentResponsibleDeputyRequest)
    - [AssignDepartmentResponsibleDeputyResponse](#medincident-service-membership-v1-AssignDepartmentResponsibleDeputyResponse)
    - [AssignDepartmentResponsibleRequest](#medincident-service-membership-v1-AssignDepartmentResponsibleRequest)
    - [AssignDepartmentResponsibleResponse](#medincident-service-membership-v1-AssignDepartmentResponsibleResponse)
    - [AssignOrganizationAdminDeputyRequest](#medincident-service-membership-v1-AssignOrganizationAdminDeputyRequest)
    - [AssignOrganizationAdminDeputyResponse](#medincident-service-membership-v1-AssignOrganizationAdminDeputyResponse)
    - [AssignOrganizationAdminRequest](#medincident-service-membership-v1-AssignOrganizationAdminRequest)
    - [AssignOrganizationAdminResponse](#medincident-service-membership-v1-AssignOrganizationAdminResponse)
    - [AssignOrganizationDispatcherDeputyRequest](#medincident-service-membership-v1-AssignOrganizationDispatcherDeputyRequest)
    - [AssignOrganizationDispatcherDeputyResponse](#medincident-service-membership-v1-AssignOrganizationDispatcherDeputyResponse)
    - [AssignOrganizationDispatcherRequest](#medincident-service-membership-v1-AssignOrganizationDispatcherRequest)
    - [AssignOrganizationDispatcherResponse](#medincident-service-membership-v1-AssignOrganizationDispatcherResponse)
    - [AssignOrganizationHeadDeputyRequest](#medincident-service-membership-v1-AssignOrganizationHeadDeputyRequest)
    - [AssignOrganizationHeadDeputyResponse](#medincident-service-membership-v1-AssignOrganizationHeadDeputyResponse)
    - [AssignOrganizationHeadRequest](#medincident-service-membership-v1-AssignOrganizationHeadRequest)
    - [AssignOrganizationHeadResponse](#medincident-service-membership-v1-AssignOrganizationHeadResponse)
    - [CancelScheduledVacationRequest](#medincident-service-membership-v1-CancelScheduledVacationRequest)
    - [CancelScheduledVacationResponse](#medincident-service-membership-v1-CancelScheduledVacationResponse)
    - [ForceEndVacationRequest](#medincident-service-membership-v1-ForceEndVacationRequest)
    - [ForceEndVacationResponse](#medincident-service-membership-v1-ForceEndVacationResponse)
    - [GrantSystemAdminRequest](#medincident-service-membership-v1-GrantSystemAdminRequest)
    - [GrantSystemAdminResponse](#medincident-service-membership-v1-GrantSystemAdminResponse)
    - [HireEmployeeRequest](#medincident-service-membership-v1-HireEmployeeRequest)
    - [HireEmployeeResponse](#medincident-service-membership-v1-HireEmployeeResponse)
    - [RemoveClinicHeadDeputyRequest](#medincident-service-membership-v1-RemoveClinicHeadDeputyRequest)
    - [RemoveClinicHeadDeputyResponse](#medincident-service-membership-v1-RemoveClinicHeadDeputyResponse)
    - [RemoveDepartmentResponsibleDeputyRequest](#medincident-service-membership-v1-RemoveDepartmentResponsibleDeputyRequest)
    - [RemoveDepartmentResponsibleDeputyResponse](#medincident-service-membership-v1-RemoveDepartmentResponsibleDeputyResponse)
    - [RemoveOrganizationAdminDeputyRequest](#medincident-service-membership-v1-RemoveOrganizationAdminDeputyRequest)
    - [RemoveOrganizationAdminDeputyResponse](#medincident-service-membership-v1-RemoveOrganizationAdminDeputyResponse)
    - [RemoveOrganizationDispatcherDeputyRequest](#medincident-service-membership-v1-RemoveOrganizationDispatcherDeputyRequest)
    - [RemoveOrganizationDispatcherDeputyResponse](#medincident-service-membership-v1-RemoveOrganizationDispatcherDeputyResponse)
    - [RemoveOrganizationHeadDeputyRequest](#medincident-service-membership-v1-RemoveOrganizationHeadDeputyRequest)
    - [RemoveOrganizationHeadDeputyResponse](#medincident-service-membership-v1-RemoveOrganizationHeadDeputyResponse)
    - [RevokeClinicHeadRequest](#medincident-service-membership-v1-RevokeClinicHeadRequest)
    - [RevokeClinicHeadResponse](#medincident-service-membership-v1-RevokeClinicHeadResponse)
    - [RevokeDepartmentResponsibleRequest](#medincident-service-membership-v1-RevokeDepartmentResponsibleRequest)
    - [RevokeDepartmentResponsibleResponse](#medincident-service-membership-v1-RevokeDepartmentResponsibleResponse)
    - [RevokeOrganizationAdminRequest](#medincident-service-membership-v1-RevokeOrganizationAdminRequest)
    - [RevokeOrganizationAdminResponse](#medincident-service-membership-v1-RevokeOrganizationAdminResponse)
    - [RevokeOrganizationDispatcherRequest](#medincident-service-membership-v1-RevokeOrganizationDispatcherRequest)
    - [RevokeOrganizationDispatcherResponse](#medincident-service-membership-v1-RevokeOrganizationDispatcherResponse)
    - [RevokeOrganizationHeadRequest](#medincident-service-membership-v1-RevokeOrganizationHeadRequest)
    - [RevokeOrganizationHeadResponse](#medincident-service-membership-v1-RevokeOrganizationHeadResponse)
    - [RevokeSystemAdminRequest](#medincident-service-membership-v1-RevokeSystemAdminRequest)
    - [RevokeSystemAdminResponse](#medincident-service-membership-v1-RevokeSystemAdminResponse)
    - [ScheduleVacationRequest](#medincident-service-membership-v1-ScheduleVacationRequest)
    - [ScheduleVacationResponse](#medincident-service-membership-v1-ScheduleVacationResponse)
    - [StartVacationNowRequest](#medincident-service-membership-v1-StartVacationNowRequest)
    - [StartVacationNowResponse](#medincident-service-membership-v1-StartVacationNowResponse)
    - [TerminateEmployeeRequest](#medincident-service-membership-v1-TerminateEmployeeRequest)
    - [TerminateEmployeeResponse](#medincident-service-membership-v1-TerminateEmployeeResponse)
    - [UpdateEmployeeDepartmentRequest](#medincident-service-membership-v1-UpdateEmployeeDepartmentRequest)
    - [UpdateEmployeeDepartmentResponse](#medincident-service-membership-v1-UpdateEmployeeDepartmentResponse)
    - [UpdateEmployeePositionRequest](#medincident-service-membership-v1-UpdateEmployeePositionRequest)
    - [UpdateEmployeePositionResponse](#medincident-service-membership-v1-UpdateEmployeePositionResponse)
    - [UpdateVacationEndDateRequest](#medincident-service-membership-v1-UpdateVacationEndDateRequest)
    - [UpdateVacationEndDateResponse](#medincident-service-membership-v1-UpdateVacationEndDateResponse)

    - [MembershipService](#medincident-service-membership-v1-MembershipService)

- [Scalar Value Types](#scalar-value-types)



<a name="medincident_service_membership_v1_membership-proto"></a>
<p align="right"><a href="#top">Top</a></p>

## medincident/service/membership/v1/membership.proto



<a name="medincident-service-membership-v1-AssignClinicHeadDeputyRequest"></a>

### AssignClinicHeadDeputyRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| clinic_id | [string](#string) |  |  |
| employee_id | [string](#string) |  |  |
| deputy_employee_id | [string](#string) |  |  |






<a name="medincident-service-membership-v1-AssignClinicHeadDeputyResponse"></a>

### AssignClinicHeadDeputyResponse







<a name="medincident-service-membership-v1-AssignClinicHeadRequest"></a>

### AssignClinicHeadRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| clinic_id | [string](#string) |  |  |
| employee_id | [string](#string) |  |  |






<a name="medincident-service-membership-v1-AssignClinicHeadResponse"></a>

### AssignClinicHeadResponse







<a name="medincident-service-membership-v1-AssignDepartmentResponsibleDeputyRequest"></a>

### AssignDepartmentResponsibleDeputyRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| department_id | [string](#string) |  |  |
| employee_id | [string](#string) |  |  |
| deputy_employee_id | [string](#string) |  |  |






<a name="medincident-service-membership-v1-AssignDepartmentResponsibleDeputyResponse"></a>

### AssignDepartmentResponsibleDeputyResponse







<a name="medincident-service-membership-v1-AssignDepartmentResponsibleRequest"></a>

### AssignDepartmentResponsibleRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| department_id | [string](#string) |  |  |
| employee_id | [string](#string) |  |  |






<a name="medincident-service-membership-v1-AssignDepartmentResponsibleResponse"></a>

### AssignDepartmentResponsibleResponse







<a name="medincident-service-membership-v1-AssignOrganizationAdminDeputyRequest"></a>

### AssignOrganizationAdminDeputyRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| organization_id | [string](#string) |  |  |
| employee_id | [string](#string) |  |  |
| deputy_employee_id | [string](#string) |  |  |






<a name="medincident-service-membership-v1-AssignOrganizationAdminDeputyResponse"></a>

### AssignOrganizationAdminDeputyResponse







<a name="medincident-service-membership-v1-AssignOrganizationAdminRequest"></a>

### AssignOrganizationAdminRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| organization_id | [string](#string) |  |  |
| employee_id | [string](#string) |  |  |






<a name="medincident-service-membership-v1-AssignOrganizationAdminResponse"></a>

### AssignOrganizationAdminResponse







<a name="medincident-service-membership-v1-AssignOrganizationDispatcherDeputyRequest"></a>

### AssignOrganizationDispatcherDeputyRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| organization_id | [string](#string) |  |  |
| employee_id | [string](#string) |  |  |
| deputy_employee_id | [string](#string) |  |  |






<a name="medincident-service-membership-v1-AssignOrganizationDispatcherDeputyResponse"></a>

### AssignOrganizationDispatcherDeputyResponse







<a name="medincident-service-membership-v1-AssignOrganizationDispatcherRequest"></a>

### AssignOrganizationDispatcherRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| organization_id | [string](#string) |  |  |
| employee_id | [string](#string) |  |  |






<a name="medincident-service-membership-v1-AssignOrganizationDispatcherResponse"></a>

### AssignOrganizationDispatcherResponse







<a name="medincident-service-membership-v1-AssignOrganizationHeadDeputyRequest"></a>

### AssignOrganizationHeadDeputyRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| organization_id | [string](#string) |  |  |
| employee_id | [string](#string) |  |  |
| deputy_employee_id | [string](#string) |  |  |






<a name="medincident-service-membership-v1-AssignOrganizationHeadDeputyResponse"></a>

### AssignOrganizationHeadDeputyResponse







<a name="medincident-service-membership-v1-AssignOrganizationHeadRequest"></a>

### AssignOrganizationHeadRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| organization_id | [string](#string) |  |  |
| employee_id | [string](#string) |  |  |






<a name="medincident-service-membership-v1-AssignOrganizationHeadResponse"></a>

### AssignOrganizationHeadResponse







<a name="medincident-service-membership-v1-CancelScheduledVacationRequest"></a>

### CancelScheduledVacationRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| vacation_id | [string](#string) |  |  |






<a name="medincident-service-membership-v1-CancelScheduledVacationResponse"></a>

### CancelScheduledVacationResponse







<a name="medincident-service-membership-v1-ForceEndVacationRequest"></a>

### ForceEndVacationRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| vacation_id | [string](#string) |  |  |






<a name="medincident-service-membership-v1-ForceEndVacationResponse"></a>

### ForceEndVacationResponse







<a name="medincident-service-membership-v1-GrantSystemAdminRequest"></a>

### GrantSystemAdminRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| zitadel_user_id | [string](#string) |  |  |






<a name="medincident-service-membership-v1-GrantSystemAdminResponse"></a>

### GrantSystemAdminResponse







<a name="medincident-service-membership-v1-HireEmployeeRequest"></a>

### HireEmployeeRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| zitadel_user_id | [string](#string) |  |  |
| department_id | [string](#string) |  |  |
| position | [string](#string) | optional |  |






<a name="medincident-service-membership-v1-HireEmployeeResponse"></a>

### HireEmployeeResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| employee_id | [string](#string) |  |  |






<a name="medincident-service-membership-v1-RemoveClinicHeadDeputyRequest"></a>

### RemoveClinicHeadDeputyRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| clinic_id | [string](#string) |  |  |
| employee_id | [string](#string) |  |  |






<a name="medincident-service-membership-v1-RemoveClinicHeadDeputyResponse"></a>

### RemoveClinicHeadDeputyResponse







<a name="medincident-service-membership-v1-RemoveDepartmentResponsibleDeputyRequest"></a>

### RemoveDepartmentResponsibleDeputyRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| department_id | [string](#string) |  |  |
| employee_id | [string](#string) |  |  |






<a name="medincident-service-membership-v1-RemoveDepartmentResponsibleDeputyResponse"></a>

### RemoveDepartmentResponsibleDeputyResponse







<a name="medincident-service-membership-v1-RemoveOrganizationAdminDeputyRequest"></a>

### RemoveOrganizationAdminDeputyRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| organization_id | [string](#string) |  |  |
| employee_id | [string](#string) |  |  |






<a name="medincident-service-membership-v1-RemoveOrganizationAdminDeputyResponse"></a>

### RemoveOrganizationAdminDeputyResponse







<a name="medincident-service-membership-v1-RemoveOrganizationDispatcherDeputyRequest"></a>

### RemoveOrganizationDispatcherDeputyRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| organization_id | [string](#string) |  |  |
| employee_id | [string](#string) |  |  |






<a name="medincident-service-membership-v1-RemoveOrganizationDispatcherDeputyResponse"></a>

### RemoveOrganizationDispatcherDeputyResponse







<a name="medincident-service-membership-v1-RemoveOrganizationHeadDeputyRequest"></a>

### RemoveOrganizationHeadDeputyRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| organization_id | [string](#string) |  |  |
| employee_id | [string](#string) |  |  |






<a name="medincident-service-membership-v1-RemoveOrganizationHeadDeputyResponse"></a>

### RemoveOrganizationHeadDeputyResponse







<a name="medincident-service-membership-v1-RevokeClinicHeadRequest"></a>

### RevokeClinicHeadRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| clinic_id | [string](#string) |  |  |
| employee_id | [string](#string) |  |  |






<a name="medincident-service-membership-v1-RevokeClinicHeadResponse"></a>

### RevokeClinicHeadResponse







<a name="medincident-service-membership-v1-RevokeDepartmentResponsibleRequest"></a>

### RevokeDepartmentResponsibleRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| department_id | [string](#string) |  |  |
| employee_id | [string](#string) |  |  |






<a name="medincident-service-membership-v1-RevokeDepartmentResponsibleResponse"></a>

### RevokeDepartmentResponsibleResponse







<a name="medincident-service-membership-v1-RevokeOrganizationAdminRequest"></a>

### RevokeOrganizationAdminRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| organization_id | [string](#string) |  |  |
| employee_id | [string](#string) |  |  |






<a name="medincident-service-membership-v1-RevokeOrganizationAdminResponse"></a>

### RevokeOrganizationAdminResponse







<a name="medincident-service-membership-v1-RevokeOrganizationDispatcherRequest"></a>

### RevokeOrganizationDispatcherRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| organization_id | [string](#string) |  |  |
| employee_id | [string](#string) |  |  |






<a name="medincident-service-membership-v1-RevokeOrganizationDispatcherResponse"></a>

### RevokeOrganizationDispatcherResponse







<a name="medincident-service-membership-v1-RevokeOrganizationHeadRequest"></a>

### RevokeOrganizationHeadRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| organization_id | [string](#string) |  |  |
| employee_id | [string](#string) |  |  |






<a name="medincident-service-membership-v1-RevokeOrganizationHeadResponse"></a>

### RevokeOrganizationHeadResponse







<a name="medincident-service-membership-v1-RevokeSystemAdminRequest"></a>

### RevokeSystemAdminRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| zitadel_user_id | [string](#string) |  |  |






<a name="medincident-service-membership-v1-RevokeSystemAdminResponse"></a>

### RevokeSystemAdminResponse







<a name="medincident-service-membership-v1-ScheduleVacationRequest"></a>

### ScheduleVacationRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| employee_id | [string](#string) |  |  |
| starts_at | [google.protobuf.Timestamp](#google-protobuf-Timestamp) |  |  |
| ends_at | [google.protobuf.Timestamp](#google-protobuf-Timestamp) | optional |  |






<a name="medincident-service-membership-v1-ScheduleVacationResponse"></a>

### ScheduleVacationResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| vacation_id | [string](#string) |  |  |






<a name="medincident-service-membership-v1-StartVacationNowRequest"></a>

### StartVacationNowRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| employee_id | [string](#string) |  |  |
| ends_at | [google.protobuf.Timestamp](#google-protobuf-Timestamp) | optional |  |






<a name="medincident-service-membership-v1-StartVacationNowResponse"></a>

### StartVacationNowResponse



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| vacation_id | [string](#string) |  |  |






<a name="medincident-service-membership-v1-TerminateEmployeeRequest"></a>

### TerminateEmployeeRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| employee_id | [string](#string) |  |  |






<a name="medincident-service-membership-v1-TerminateEmployeeResponse"></a>

### TerminateEmployeeResponse







<a name="medincident-service-membership-v1-UpdateEmployeeDepartmentRequest"></a>

### UpdateEmployeeDepartmentRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| employee_id | [string](#string) |  |  |
| department_id | [string](#string) |  |  |






<a name="medincident-service-membership-v1-UpdateEmployeeDepartmentResponse"></a>

### UpdateEmployeeDepartmentResponse







<a name="medincident-service-membership-v1-UpdateEmployeePositionRequest"></a>

### UpdateEmployeePositionRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| employee_id | [string](#string) |  |  |
| position | [string](#string) | optional |  |






<a name="medincident-service-membership-v1-UpdateEmployeePositionResponse"></a>

### UpdateEmployeePositionResponse







<a name="medincident-service-membership-v1-UpdateVacationEndDateRequest"></a>

### UpdateVacationEndDateRequest



| Field | Type | Label | Description |
| ----- | ---- | ----- | ----------- |
| vacation_id | [string](#string) |  |  |
| ends_at | [google.protobuf.Timestamp](#google-protobuf-Timestamp) |  |  |






<a name="medincident-service-membership-v1-UpdateVacationEndDateResponse"></a>

### UpdateVacationEndDateResponse













<a name="medincident-service-membership-v1-MembershipService"></a>

### MembershipService
MembershipService is the command-side contract for who-works-where
data. It covers the Employee lifecycle (hire / update / terminate),
Vacation lifecycle (start / schedule / end / cancel / change),
role grants with deputy management for DepartmentResponsible,
ClinicHead, OrganizationAdmin, OrganizationHead, and
OrganizationDispatcher, plus the global SystemAdmin grant that
operates directly on Zitadel user identifiers.

| Method Name | Request Type | Response Type | Description |
| ----------- | ------------ | ------------- | ------------|
| HireEmployee | [HireEmployeeRequest](#medincident-service-membership-v1-HireEmployeeRequest) | [HireEmployeeResponse](#medincident-service-membership-v1-HireEmployeeResponse) | Employee lifecycle |
| UpdateEmployeePosition | [UpdateEmployeePositionRequest](#medincident-service-membership-v1-UpdateEmployeePositionRequest) | [UpdateEmployeePositionResponse](#medincident-service-membership-v1-UpdateEmployeePositionResponse) |  |
| UpdateEmployeeDepartment | [UpdateEmployeeDepartmentRequest](#medincident-service-membership-v1-UpdateEmployeeDepartmentRequest) | [UpdateEmployeeDepartmentResponse](#medincident-service-membership-v1-UpdateEmployeeDepartmentResponse) |  |
| TerminateEmployee | [TerminateEmployeeRequest](#medincident-service-membership-v1-TerminateEmployeeRequest) | [TerminateEmployeeResponse](#medincident-service-membership-v1-TerminateEmployeeResponse) |  |
| StartVacationNow | [StartVacationNowRequest](#medincident-service-membership-v1-StartVacationNowRequest) | [StartVacationNowResponse](#medincident-service-membership-v1-StartVacationNowResponse) | Vacation lifecycle |
| ScheduleVacation | [ScheduleVacationRequest](#medincident-service-membership-v1-ScheduleVacationRequest) | [ScheduleVacationResponse](#medincident-service-membership-v1-ScheduleVacationResponse) |  |
| UpdateVacationEndDate | [UpdateVacationEndDateRequest](#medincident-service-membership-v1-UpdateVacationEndDateRequest) | [UpdateVacationEndDateResponse](#medincident-service-membership-v1-UpdateVacationEndDateResponse) |  |
| ForceEndVacation | [ForceEndVacationRequest](#medincident-service-membership-v1-ForceEndVacationRequest) | [ForceEndVacationResponse](#medincident-service-membership-v1-ForceEndVacationResponse) |  |
| CancelScheduledVacation | [CancelScheduledVacationRequest](#medincident-service-membership-v1-CancelScheduledVacationRequest) | [CancelScheduledVacationResponse](#medincident-service-membership-v1-CancelScheduledVacationResponse) |  |
| AssignDepartmentResponsible | [AssignDepartmentResponsibleRequest](#medincident-service-membership-v1-AssignDepartmentResponsibleRequest) | [AssignDepartmentResponsibleResponse](#medincident-service-membership-v1-AssignDepartmentResponsibleResponse) | ------ DepartmentResponsible ------ |
| RevokeDepartmentResponsible | [RevokeDepartmentResponsibleRequest](#medincident-service-membership-v1-RevokeDepartmentResponsibleRequest) | [RevokeDepartmentResponsibleResponse](#medincident-service-membership-v1-RevokeDepartmentResponsibleResponse) |  |
| AssignDepartmentResponsibleDeputy | [AssignDepartmentResponsibleDeputyRequest](#medincident-service-membership-v1-AssignDepartmentResponsibleDeputyRequest) | [AssignDepartmentResponsibleDeputyResponse](#medincident-service-membership-v1-AssignDepartmentResponsibleDeputyResponse) |  |
| RemoveDepartmentResponsibleDeputy | [RemoveDepartmentResponsibleDeputyRequest](#medincident-service-membership-v1-RemoveDepartmentResponsibleDeputyRequest) | [RemoveDepartmentResponsibleDeputyResponse](#medincident-service-membership-v1-RemoveDepartmentResponsibleDeputyResponse) |  |
| AssignClinicHead | [AssignClinicHeadRequest](#medincident-service-membership-v1-AssignClinicHeadRequest) | [AssignClinicHeadResponse](#medincident-service-membership-v1-AssignClinicHeadResponse) | ------ ClinicHead ------ |
| RevokeClinicHead | [RevokeClinicHeadRequest](#medincident-service-membership-v1-RevokeClinicHeadRequest) | [RevokeClinicHeadResponse](#medincident-service-membership-v1-RevokeClinicHeadResponse) |  |
| AssignClinicHeadDeputy | [AssignClinicHeadDeputyRequest](#medincident-service-membership-v1-AssignClinicHeadDeputyRequest) | [AssignClinicHeadDeputyResponse](#medincident-service-membership-v1-AssignClinicHeadDeputyResponse) |  |
| RemoveClinicHeadDeputy | [RemoveClinicHeadDeputyRequest](#medincident-service-membership-v1-RemoveClinicHeadDeputyRequest) | [RemoveClinicHeadDeputyResponse](#medincident-service-membership-v1-RemoveClinicHeadDeputyResponse) |  |
| AssignOrganizationAdmin | [AssignOrganizationAdminRequest](#medincident-service-membership-v1-AssignOrganizationAdminRequest) | [AssignOrganizationAdminResponse](#medincident-service-membership-v1-AssignOrganizationAdminResponse) | ------ OrganizationAdmin ------ |
| RevokeOrganizationAdmin | [RevokeOrganizationAdminRequest](#medincident-service-membership-v1-RevokeOrganizationAdminRequest) | [RevokeOrganizationAdminResponse](#medincident-service-membership-v1-RevokeOrganizationAdminResponse) |  |
| AssignOrganizationAdminDeputy | [AssignOrganizationAdminDeputyRequest](#medincident-service-membership-v1-AssignOrganizationAdminDeputyRequest) | [AssignOrganizationAdminDeputyResponse](#medincident-service-membership-v1-AssignOrganizationAdminDeputyResponse) |  |
| RemoveOrganizationAdminDeputy | [RemoveOrganizationAdminDeputyRequest](#medincident-service-membership-v1-RemoveOrganizationAdminDeputyRequest) | [RemoveOrganizationAdminDeputyResponse](#medincident-service-membership-v1-RemoveOrganizationAdminDeputyResponse) |  |
| AssignOrganizationHead | [AssignOrganizationHeadRequest](#medincident-service-membership-v1-AssignOrganizationHeadRequest) | [AssignOrganizationHeadResponse](#medincident-service-membership-v1-AssignOrganizationHeadResponse) | ------ OrganizationHead ------ |
| RevokeOrganizationHead | [RevokeOrganizationHeadRequest](#medincident-service-membership-v1-RevokeOrganizationHeadRequest) | [RevokeOrganizationHeadResponse](#medincident-service-membership-v1-RevokeOrganizationHeadResponse) |  |
| AssignOrganizationHeadDeputy | [AssignOrganizationHeadDeputyRequest](#medincident-service-membership-v1-AssignOrganizationHeadDeputyRequest) | [AssignOrganizationHeadDeputyResponse](#medincident-service-membership-v1-AssignOrganizationHeadDeputyResponse) |  |
| RemoveOrganizationHeadDeputy | [RemoveOrganizationHeadDeputyRequest](#medincident-service-membership-v1-RemoveOrganizationHeadDeputyRequest) | [RemoveOrganizationHeadDeputyResponse](#medincident-service-membership-v1-RemoveOrganizationHeadDeputyResponse) |  |
| AssignOrganizationDispatcher | [AssignOrganizationDispatcherRequest](#medincident-service-membership-v1-AssignOrganizationDispatcherRequest) | [AssignOrganizationDispatcherResponse](#medincident-service-membership-v1-AssignOrganizationDispatcherResponse) | ------ OrganizationDispatcher ------ |
| RevokeOrganizationDispatcher | [RevokeOrganizationDispatcherRequest](#medincident-service-membership-v1-RevokeOrganizationDispatcherRequest) | [RevokeOrganizationDispatcherResponse](#medincident-service-membership-v1-RevokeOrganizationDispatcherResponse) |  |
| AssignOrganizationDispatcherDeputy | [AssignOrganizationDispatcherDeputyRequest](#medincident-service-membership-v1-AssignOrganizationDispatcherDeputyRequest) | [AssignOrganizationDispatcherDeputyResponse](#medincident-service-membership-v1-AssignOrganizationDispatcherDeputyResponse) |  |
| RemoveOrganizationDispatcherDeputy | [RemoveOrganizationDispatcherDeputyRequest](#medincident-service-membership-v1-RemoveOrganizationDispatcherDeputyRequest) | [RemoveOrganizationDispatcherDeputyResponse](#medincident-service-membership-v1-RemoveOrganizationDispatcherDeputyResponse) |  |
| GrantSystemAdmin | [GrantSystemAdminRequest](#medincident-service-membership-v1-GrantSystemAdminRequest) | [GrantSystemAdminResponse](#medincident-service-membership-v1-GrantSystemAdminResponse) | ------ SystemAdmin ------ |
| RevokeSystemAdmin | [RevokeSystemAdminRequest](#medincident-service-membership-v1-RevokeSystemAdminRequest) | [RevokeSystemAdminResponse](#medincident-service-membership-v1-RevokeSystemAdminResponse) |  |





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
