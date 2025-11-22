# ElectroSoft Data Contract

API format , endpoints and defined request , response flow.

Written by . - Aman Kadam

# Defined Unified Response Format
All `Rest API` would follow this API response structure:
```json
{
"data": {
"response": {
"records": [],
"total_records": int
} | ANY,
"success_message": "" | string
},
"error_message": "",
"is_error": boolean
}
```
---
#  API no. 01
#### Method: `POST`
#### Name: Login (Common API for all clients/business-owner employees/customers)
#### Endpoint: `/api/auth/login`
### Request Body
```json
{
"email": "admin@electrosoft.com",
"password": "C0d3_Is_4rt_C0de_I5_U5"
}
```

### Response
```json
{
"data": {
"response": {
 "token_data": {
 "access_token": "ey-secret-access-token",
 "refresh_token": "ey-secret-refresh-token"
 },
 "is_first_login": true,
 "role":"admin",
 "user_data": {
 "first_name": "Admin",
 "last_name": "Admin",
 "email": "admin@electrosoft.com"
 },
 "login_type": electrosoft | consumer | client,
 "app_config": {
 "preferences": {
 "color_theme_id" : olive | purple | vector | grey
 },
 "app_logo_url": "cloudflare.cdn/img1.png"
 },
},
"success_message": "Login successful."
},
"error_message": "",
"is_error": false
}
```
---
#  API no. 02
#### Method: `POST`
#### Name: Change Password (Common API for all clients/business-owner employees/consumers)
#### Endpoint: `/api/auth/change-password`
### Request Body
```json
{
"old_password": "C0d3_Is_4rt_C0de_I5_U5",
"new_password": "C0D1T4S_C0d3_Is_4rt_C0de_I5_U5",
"confirm_password": "C0D1T4S_C0d3_Is_4rt_C0de_I5_U5"
}
```
### Response
```json
{
"data": {
"response": null,
"success_message": "Password changed successfully."
},
"error_message": "",
"is_error": false
}
```
---
#  API no. 03
#### Method: `GET`
#### Name: Admin Dashboard
#### Endpoint: `/api/admin/dashboard`
### No Request Body Required | `Token data sufficient to GET Response`
### Response
```json
{
"data": {
"response": {
"revenue_data": {
"total_revenue" : float,
"revenue_sources": [
{
"state_name": "Rajasthan",
"revenue_generated": 160000000
},
{
"state_name": "Maharashtra",
"revenue_generated": 890000000
}
]
},
"meter_connections": [
{
"state_name": "Rajasthan",
"total": 153000000
}
]
},
"success_message": "Data fetched successfully."
},
"error_message": "",
"is_error": false
}
```
---
#  API no. 04
#### Method: `POST`
#### Name: Get Employees
##### Features : Paginated / Filtered Responses
#### Endpoint: `/api/employees`
### Request Body
```json
{
"page_size" : int,
"page_no" : int,
"search_query" : str,
"filter_parameters": {
"state_ids" : list[int],
"district_ids" : list[int],
"city_ids" : list[int]
}
}
```
### Response
```json
{
"data": {
"response": {
"records": [
{
"employee_id": int
"employee_name": str,
"employee_role": str | enum
}
],
"total": int
},
"success_message": "Data fetched successfully."
},
"error_message": "",
"is_error": false
}
```
---
#  API no. 05
#### Method: `DELETE`
#### Name: DELETE Employee
#### Endpoint: `/api/employee/{employee_id}`
### Request Body | query_param
```json
"employee_id": int
```
### Response
```json
{
"data": {
"response" : null,
"success_message": "Employee deleted successfully."
},
"error_message": "",
"is_error": false
}
```
---
#  API no. 06
#### Method: `POST`
#### Name: Add/Create Individual Employee Info on `Add Employee`
#### Endpoint: `/api/employee`
### Request Body
```json
{
"employee_info": {
"first_name": str,
"last_name": str,
"email": str,
"phone_no": str,
"role": enum | int,
"role_hierarchy": {
"state_ids": list[int],
"district_ids": list[int],
"city_ids": list[int]
},
"address": str
}
}
```
### Response
```json
{
"data": {
"response" : null,
"success_message": "Employee added successfully."
},
"error_message": "",
"is_error": false
}   
```
---
#  API no. 07
#### Method: `GET`
#### Name: Get Individual Employee Info on `EDIT`
#### Endpoint: `/api/employee/{employee_id}`
### Request Body | query_param
```json
"employee_id": int
```
### Response
```json
{
"data": {
"response" : {
"employee_info": {
"employee_id": int,
"first_name": str,
"last_name": str,
"email": str,
"phone_no": str,
"role": enum | int,
"role_hierarchy": {
"state_ids": list[int],
"district_ids": list[int],
"city_ids": list[int]
},
"address": str
}
},
"success_message": "Data fetched successfully."
},
"error_message": "",
"is_error": false
}
```
---
#  API no. 08
#### Method: `PATCH`
#### Name: Edit/Update Individual Employee Info on `Edit Employee`
#### Endpoint: `/api/employee`
### Request Body
```json
{
"employee_info": {
"employee_id": int,
"first_name": str,
"last_name": str,
"email": str,
"phone_no": str,
"role": enum | int,
"role_hierarchy": {
"state_ids": list[int],
"district_ids": list[int],
"city_ids": list[int]
},
"address": str
}
}
```
### Response
```json
{
"data": {
"response" : null,
"success_message": "Employee updated successfully."
},
"error_message": "",
"is_error": false
}   
```
---
#  API no. 09
#### Method: `POST`
#### Name: Get Operational Locations for Dropdown Population of Edit Employee 
#### `(Dependency Dropdown)`
#### Endpoint: `/api/operational-for-employee-edit`
### Request Body
```json
{
"employee_id": int | null,
"state_ids": [
{
"state_id": int
}
],
"district_ids": [
{
"parent_state_id": int,
"district_id": int
}
],
"city_ids": [
{
"parent_district_id": int,
"city_id": int
}
]
}
```
### Response
```json
{
"data": {
"response" : {
"state_ids": [
{
"state_id": int,
"state_name": str
}
],
"district_ids": [
{
"parent_state_id": int,
"district_id": int,
"district_name": str
}
],
"city_ids": [
{
"parent_district_id": int,
"city_id": int,
"city_name": str
}
]
},
"success_message": "Data fetched successfully."
},
"error_message": "",
"is_error": false
}
```
---
#  API no. 10
#### Method: `POST`
#### Name: Get Operational Locations for ElectroSoft / Service Providers
#### Feature: Can be used for both Modules - `ElectroSoft` as well as `Service Provider`
#### Hybrid API: Would return the differential responses based on availability of service_provider_id into token either `None` or `str(ID)`
#### Endpoint: `/api/operational`
### Request Body
```json
{
"state_ids": [
{
"state_id": int
}
],
"district_ids": [
{
"parent_state_id": int,
"district_id": int
}
],
"city_ids": [
{
"parent_district_id": int,
"city_id": int
}
]
}
```
### Response
```json
{
"data": {
"response" : {
"records": [
{
"city_id" : int,
"city_name": str,
"district_name": str,
"state_name" : str
}
],
"total": int
},
"success_message": "Data fetched successfully."
},
"error_message": "",
"is_error": false
}  
```
---
#  API no. 10
#### Method: `DELETE`
#### Name: Get Operational Locations for ElectroSoft / Service Providers
#### Feature: Can be used for both Modules - `ElectroSoft` as well as `Service Provider`
#### Hybrid API: Would return the differential responses based on availability of service_provider_id into token either `None` or `str(ID)`
#### Endpoint: `/api/operational`
### Request Body
```json

```
---
# API no. 11
#### Method: `POST`
#### Name: Refresh Login
#### Feature: Can be used for All  Modules - `ElectroSoft` as well as ` Service Provider and Consumer
#### Endpoint: `/api/refresh-login`
### Request Body
```json
{
"refresh_token" : "ey-refresh-token" 
}
```

### Response
```json
{
"data": {
"response": {
 "token_data": {
 "access_token": "ey-secret-access-token",
 "refresh_token": "ey-secret-refresh-token"
 },
 "is_first_login": true,
 "role":"admin",
 "user_data": {
 "first_name": "Admin",
 "last_name": "Admin",
 "email": "admin@electrosoft.com"
 },
 "login_type": electrosoft | consumer | client,
 "app_config": {
 "preferences": {
 "color_theme_id" : olive | purple | vector | grey
 },
 "app_logo_url": "cloudflare.cdn/img1.png"
 },
},
"success_message": "Login Refresh successful."
},
"error_message": "",
"is_error": false
}
```
---
# API no. 12
#### Method: `POST`
#### Name: Get Service Provider Paginated / searched List 
#### Feature: Can be used for Module - `ElectroSoft`
#### Endpoint: `/api/get/service-provider`
### Request Body
```json
{
"page_size" : int,
"page_no" : int,
"search_query" : str,
"filter_parameters": null
}
```
### Response
```json
{
"data": {
"response" : {
"records": [
{
"provider_name" : str,
"revenue": int,
"no_of_users": int
}
],
"total": int
},
"success_message": "Data fetched successfully."
},
"error_message": "",
"is_error": false
}  
```
---
# API no. 13
#### Method: `GET`
#### Name: Get Service Provider info on Click of Row. 
#### Feature: Can be used for Module - `ElectroSoft` | Admin
#### Endpoint: `/api/get/service-provider/{service_provider_id}`
### Request Body | query_param
```json
"service_provider_id": int
```
### Response
```json
{
"data": {
"response" : {
"provider_name" : str,
"provider_plan" : str,
"status": boolean,
"representative_managers_list": [
{
"manager_id": int,
"manager_name": str
}
]
}
},
"success_message": "Data fetched successfully."
},
"error_message": "",
"is_error": false
} 
```
---
# API no. 14
#### Method: `PATCH`
#### Name: Revoke Client Manager of a Service Provider. 
#### Feature: Can be used for Module - `ElectroSoft` | Admins
#### Endpoint: `/api/revoke-service-provider-manager`
### Request Body
```json
{
"service_provider_id": int,
"client_manager_id": int
}
```

### Response
```json
{
"data": {
"response" :null,
"success_message": "Client manager revoked successfully."
},
"error_message": "",
"is_error": false
} 
```
---
# API no. 15
#### Method: `POST`
#### Name: Get Client Managers List. 
#### Feature: Can be used for Module - `ElectroSoft` | Admins | Heads
#### Endpoint: `/api/service-provider-managers`
### Request Body
```json
{
"page_size" : int,
"page_no" : int,
"search_query" : str,
"filter_parameters": null
}
```
### Response
```json
{
{
"data": {
"response" : {
"records": [
{
"client_manager_id": int,
"client_manager_name": str,
"service_provider_name": str
}
],
"total": int
},
"success_message": "Data fetched successfully."
},
"error_message": "",
"is_error": false
}  
```
---
# API no. 16
#### Method: `POST`
#### Name: Get Subscription Plan List. 
#### Feature: Can be used for Module - `ElectroSoft` | Admins
#### Endpoint: `/api/subscription-plans`
### Request Body
```json
{
"page_size" : int,
"page_no" : int,
"search_query" : str,
"filter_parameters": null
}
```
### Response
```json
{
{
"data": {
"response" : {
"records": [
{
"subscription_id": int,
"subscription_name": str,
"range_in_users": str,
"validity": int,
"price": float
}
],
"total": int
},
"success_message": "Data fetched successfully."
},
"error_message": "",
"is_error": false
}
```
---
# API no. 17
#### Method: `POST`
#### Name: Add Subscription Plan. 
#### Feature: Can be used for Module - `ElectroSoft` | Admins
#### Endpoint: `/api/subscription-plan`
### Request Body
```json
{
"name": str,
"validity": int,
"min_users": int,
"max_users": int, 
"price": float,
}
```
### Response
```json
{
{
"data": {
"response" : null,
"success_message": "Subscription added successfully."
},
"error_message": "",
"is_error": false
}
```
---
# API no. 18
#### Method: `GET`
#### Name: Get Subscription Plan Individual for Edit Pre-population of Form. 
#### Feature: Can be used for Module - `ElectroSoft` | Admins
#### Endpoint: `/api/subscription-plan/{plan_id}`
### Request Body | Query_Param
```json
"plan_id": int
```
### Response
```json
{
{
"data": {
"response" : {
"plan_id": int,
"name": str,
"validity": int,
"min_users": int,
"max_users": int, 
"price": float,
},
"success_message": "Data fetched successfully."
},
"error_message": "",
"is_error": false
}
```
---
# API no. 19
#### Method: `PATCH`
#### Name: EDIT Subscription Plan. 
#### Feature: Can be used for Module - `ElectroSoft` | Admins
#### Endpoint: `/api/subscription-plan`
### Request Body
```json
{
"plan_id": int,
"name": str,
"validity": int,
"min_users": int,
"max_users": int, 
"price": float,
}
```
### Response
```json
{
{
"data": {
"response" : null,
"success_message": "Subscription updated successfully."
},
"error_message": "",
"is_error": false
}
```
---
# API no. 20
#### Method: `POST`
#### Name: Get  Service Providers List. 
#### Feature: Can be used for Module - `ElectroSoft` | Admins
#### Endpoint: `/api/subscription-plans`
### Request Body
```json
{
"page_size" : int,
"page_no" : int,
"search_query" : str,
"filter_parameters": {
""
}
}
```
---
# API no. 21
#### Method: `POST`
#### Name: Get Service Provider Paginated / searched List for Manager Only 
#### Feature: Can be used for Module - `ElectroSoft`
#### Endpoint: `/api/get/service-provider`
### Request Body
```json
{
"page_size" : int,
"page_no" : int,
"search_query" : str,
"filter_parameters": {
"is_manager": boolean
}
}
```
### Response
```json
{
"data": {
"response" : {
"records": [
{
"provider_id": int,
"provider_name" : str,
"revenue": int,
"no_of_users": int
}
],
"total": int
},
"success_message": "Data fetched successfully."
},
"error_message": "",
"is_error": false
}  
```
---
# API no. 22
#### Method: `POST`
#### Name: Onboard Service Provider By Manager Position | ElectroSoft 
#### Feature: Can be used for Module - `ElectroSoft`
#### Endpoint: `/api/get/onboard-service-provider`
### Request Body
```json
{
"first_name": str,
"last_name": str,
"email": str,
"company_name": str,
"no_of_users": int,
"plan_id": int,
"company_logo": file,
"theme_enum": enum 
}
```
### Response
```json
{
"data": {
"response" : null,
"success_message": "Service Provided onboarded successfully."
},
"error_message": "",
"is_error": false
}
```
---
# API no. 23
#### Method: `GET`
#### Name: Get Service Provider By Manager Position | ElectroSoft 
#### Feature: Can be used for Module - `ElectroSoft`
#### Endpoint: `/api/get/service-provider/{service-provider-id}`
### Request Body | Query Parameter
```json
"service-provider-id": int
```
### Response
```json
{
"data": {
"response" : {
"service_provider_id": int,
"first_name": str,
"last_name": str,
"email": str,
"company_name": str,
"no_of_users": int,
"plan_id": int,
"company_logo": file,
"theme_enum": enum 
},
"success_message": "Data fetched successfully."
},
"error_message": "",
"is_error": false
}
```
---
# API no. 24
#### Method: `DELETE`
#### Name: Inactivate Service Provider By Manager Position | ElectroSoft 
#### Feature: Can be used for Module - `ElectroSoft`
#### Endpoint: `/api/get/service-provider/{service-provider-id}`
### Request Body | Query Parameter
```json
"service-provider-id": int
```
### Response
```json
{
"data": {
"response" : null,
"success_message": "Service Provider Deactivated successfully."
},
"error_message": "",
"is_error": false
}
```
---
# API no. 25
#### Method: `GET`
#### Name: Get Service Provider Invoices By Manager Position | ElectroSoft 
#### Feature: Can be used for Module - `ElectroSoft`
#### Endpoint: `/api/service-provider-invoices/{service-provider-id}`
### Request Body | Query Parameter
```json
"service-provider-id": int
```
### Response
```json
{
"data": {
"response" : {
"service_provider_id": int,
"invoice_id": int,
"invoice_date": str,
"invoice_amount": float,
"invoice_sent": boolean
},
"success_message": "Data fetched successfully."
},
"error_message": "",
"is_error": false
}
```
---
# API no. 26
#### Method: `POST`
#### Name: Generate Invoice for Service Provider By Manager Position | ElectroSoft 
#### Feature: Can be used for Module - `ElectroSoft`
#### Endpoint: `/api/generate-invoice-service-provider`
### Request Body
```json
{
"invoice_id": int,
"invoice_amount": float,
"no_of_users": int,
"plan_name": str,
"discount": int,
"comment": str
}
```
### Response
```json
{
"data": {
"response" : null,
"success_message": "Invoice generated successfully."
},
"error_message": "",
"is_error": false
}
```
---
# API no. 27
#### Method: `GET`
#### Name: GET Invoice for Service Provider By Manager Position | ElectroSoft 
#### Feature: Can be used for Module - `ElectroSoft`
#### Endpoint: `/api/invoice-service-provider/{invoice_id}/service_provider_id`
### Request Body | Query Param
```json
"invoice_id": int,
"service_provider_id": int
```
### Response
```json
{
"data": {
"response" : {
"invoice_id": int,
"invoice_amount": float,
"no_of_users": int,
"plan_name": str,
"discount": int,
"comment": str
},
"success_message": "Data Fetched successfully."
},
"error_message": "",
"is_error": false
}
```
---
# API no. 28
#### Method: `POST`
#### Name: Get List of Issues Raised Consumers
#### Feature: Can be used for Module - `ElectroSoft` | Client
#### Endpoint: `/api/get/consumer-issues`
### Request Body
```json
{
"page_size" : int,
"page_no" : int,
"search_query" : str,
"filter_parameters": null
}
```
### Response
```json
{
"data": {
"response" : {
"records": [
{
"provider_name" : str,
"meter_id": int,
"description": int,
"is_task_assigned": boolean
}
],
"total": int
},
"success_message": "Data fetched successfully."
},
"error_message": "",
"is_error": false
}  
```
---
# API no. 29
#### Method: `POST`
#### Name: Get List of Reading collected by Workers
#### Feature: Can be used for Module - `ElectroSoft`
#### Endpoint: `/api/get/service-provider`
### Request Body
```json
{
"page_size" : int,
"page_no" : int,
"search_query" : str,
"filter_parameters": null
}
```
### Response
```json
{
"data": {
"response" : {
"records": [
{
"bill_id": int,
"meter_id": int,
"workder_id": int,
"is_bill_generated": boolean
}
],
"total": int
},
"success_message": "Data fetched successfully."
},
"error_message": "",
"is_error": false
}  
```
---
# API no. 30
#### Method: `GET`
#### Name: Get Bill Info by bill ID
#### Feature: Can be used for Module - `ElectroSoft` support team
#### Endpoint: `/api/get/consumer-bill-info/{bill_id}`
### Request Body | Query Param
```json
bill_id: int
```
### Response
```json
{
"data": {
"response" : {
"meter_reading_images": [
{
"image_id": int,
"image_url": str
}
],
"previous_reading": int,
"present_reading": int,
"service_provider_name": str,
"billing_price": float,
"meter_id": int,
"meter_type": str,
"worker_comment": str
},
"success_message": "Data fetched successfully."
},
"error_message": "",
"is_error": false
}  
```
---
# API no. 31
#### Method: `POST`
#### Name: Generate Bill for Consumer
#### Feature: Can be used for Module - `ElectroSoft` support team
#### Endpoint: `/api/generate-consumer-bill`
### Request Body
```json
{
"meter_id": int,
"present_reading": int,
"billing_price": float
}
```
### Response
```json
{
"data": {
"response" : null,
"success_message": "Bill generated successfully."
},
"error_message": "",
"is_error": false
}  
```
---
# API no. 32
#### Method: `POST`
#### Name: Onboard Consumer
#### Feature: Can be used for Module - `ElectroSoft` support team
#### Endpoint: `/api/onboard-consumer`
### Request Body
```json
{
"first_name": str,
"last_name": str,
"email": str,
"phone_no": str,
"service_provider_id": int,
"meter_type_id": int,
"address": str
}
```
### Response
```json
{
"data": {
"response" : null,
"success_message": "Consumer Onboarded successfully."
},
"error_message": "",
"is_error": false
}  
```
---
# API no. 33
#### Method: `POST`
#### Name: Get List of Tasks assigned to worker
#### Feature: Can be used for Module - `ElectroSoft` | Worker
#### Endpoint: `/api/get/tasks`
### Request Body
```json
{
"page_size" : int,
"page_no" : int,
"search_query" : str,
"filter_parameters": {
"date_order": enum,
"work_type": enum
}
}
```
### Response
```json
{
"data": {
"response" : {
"records": [
{
"work_type": str,
"date": str,
"address": str,
"is_task_completed": boolean
}
],
"total": int
},
"success_message": "Data fetched successfully."
},
"error_message": "",
"is_error": false
}  
```
---
# API no. 34
#### Method: `GET`
#### Name: Get Tasks assigned to worker | view by ID
#### Feature: Can be used for Module - `ElectroSoft` | Worker
#### Endpoint: `/api/get/tasks/{task_id}`
### Request Body | Query Param
```json
"task_id": int
```
### Response
```json
{
"data": {
"response" : {
"task_type": str,
"task_date": str,
"address": str,
"meter_id": int,
"description": str,
"comment": str,
"is_task_completed": boolean,
"task_media": {
"images": [
{
"image_id": int,
"image_url": str
}
]
"max_images": int,
"min_images": int
}
},
"success_message": "Data fetched successfully."
},
"error_message": "",
"is_error": false
}  
```
---
# API no. 35
#### Method: `POST`
#### Name: Submit / complete Task | ElectroSoft
#### Feature: Can be used for Module - `ElectroSoft` | Worker
#### Endpoint: `/api/submit-tasks/{task_id}`
### Request Body
```json
 {
"task_type": str,
"task_date": str,
"address": str,
"meter_id": int,
"description": str,
"comment": str,
"task_media": {
"images": [
"image_file"
]
}
```
### Response
```json
{
"data": {
"response" : null,
"success_message": "Task completed successfully."
},
"error_message": "",
"is_error": false
}   
```
---
# API no. 36
#### Method: `POST`
#### Name: Get List of Meters added by Admin of service provider
#### Feature: Can be used for Module - `Client` | Admin
#### Endpoint: `/api/get/meter`
### Request Body
```json
{
"page_size" : int,
"page_no" : int,
"search_query" : str,
"filter_parameters": {
"meter_type": id
}
}
```
### Response
```json
{
"data": {
"response" : {
"records": [
{
"meter_id": int,
"meter_type": str,
"rate_per_unit": float
}
],
"total": int
},
"success_message": "Data fetched successfully."
},
"error_message": "",
"is_error": false
}  
```
---
# API no. 37
#### Method: `GET`
#### Name: Get Meters Info added by Admin of service provider by Meter_ID
#### Feature: Can be used for Module - `Client` | Admin
#### Endpoint: `/api/get/meter/{meter_id}`
### Request Body | Query Parameter
```json
"meter_id": int
```
### Response
```json
{
"data": {
"response" :{
"meter_id": int,
"meter_type_name": str,
"rate_per_unit": float
},
"success_message": "Data fetched successfully."
},
"error_message": "",
"is_error": false
}  
```
---
# API no. 38
#### Method: `POST`
#### Name:  Add Meter by Admin of service provider
#### Feature: Can be used for Module - `Client` | Admin
#### Endpoint: `/api/meter/add
### Request Body 
```json
{
"meter_type_name": str,
"rate_per_unit": str
}
```
### Response
```json
{
"data": {
"response" : null,
"success_message": "Meter type added successfully."
},
"error_message": "",
"is_error": false
}  
```
---
# API no. 39
#### Method: `DELETE`
#### Name:  Delete Meter by Admin of service provider
#### Feature: Can be used for Module - `Client` | Admin
#### Endpoint: `/api/meter/{meter_id}
### Request Body | Query Param
```json
"meter_id": int
```
### Response
```json
{
"data": {
"response" : null,
"success_message": "Meter type deleted successfully."
},
"error_message": "",
"is_error": false
}  
```
---
# API no. 40
#### Method: `POST`
#### Name: Get all bills paid and pending | Consumer View
#### Feature: Can be used for Module - `Consumer`
#### Endpoint: `/api/get/my-bills`
### Request Body
```json
{
"page_size" : int,
"page_no" : int,
"search_query" : str,
"filter_parameters": {
"bill_status": enum
}
}
```
### Response
```json
{
"data": {
"response" : {
"records": [
{
"meter_id": int,
"bill_generated_date": str,
"bill_paid_date": str,
"payment_mode": str,
"bill_amount": float,
"bill_paid": boolean
}
],
"total": int
},
"success_message": "Data fetched successfully."
},
"error_message": "",
"is_error": false
}  
```
---
# API no. 41
#### Method: `POST`
#### Name: Pay Bill | Consumer View
#### Feature: Can be used for Module - `Consumer`
#### Endpoint: `/api/pay/my-bills`
### Request Body
```json
{
"bill_id": int,
"meter_id": int,
"payment_mode": enum
}
```
### Response
```json
{
"data": {
"response" : null,
"success_message": "Bill payment Done. Thankyou.!"
},
"error_message": "",
"is_error": false
}  
```
---
# API no. 42
#### Method: `GET`
#### Name: Get Bill Payment Receipt | Consumer View
#### Feature: Can be used for Module - `Consumer`
#### Endpoint: `/api/view/my-bill/{bill_id}`
### Request Body | Query Param
```json
"bill_id": int
```
### Response
```json
{
"data": {
"response" : {
"bill_id": int,
"payment_date": str,
"payment_mode": str,
"bill_amount": float
},
"success_message": "Data fetched successfully."
},
"error_message": "",
"is_error": false
}
```
---
# API no. 43
#### Method: `POST`
#### Name: Apply New Connection | Consumer Facing UI
#### Feature: Can be used for Module - `Consumer`
#### Endpoint: `/api/apply-new-connection`
### Request Body
```json
{
"service_provider_id": int,
"meter_type_id": int,
"address": str
}
```
### Response
```json
{
"data": {
"response" : null,
"success_message": "Meter Connection request submitted successfully.\n Thankyou for your interest.!"
},
"error_message": "",
"is_error": false
}  
```
