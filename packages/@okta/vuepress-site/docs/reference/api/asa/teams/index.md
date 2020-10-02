---
title: ASA Teams
category: asa
---

# ASA Teams API

## Get Started


| Product  | API Basics  | API Namespace        |
|----------|-------------|----------------------|
| [Advanced Server Access](https://www.okta.com/products/advanced-server-access/) | [How the ASA API works](../introduction/) | `https://app.scaleft.com/v1/`

An Advanced Server Access (ASA) Team is the top-level organizational concept in ASA. Each ASA Team maps to a single app in the Okta dashboard.

All other configuration objects in Advanced Server Access are scoped to an ASA Team.

Explore the Teams API: [![Run in Postman](https://run.pstmn.io/button.svg)](https://www.getpostman.com/run-collection/fba803e43a4ae53667d4).


## Teams API Operations


The Teams API has the following operations:
* [List Servers for a Team](#list-servers-for-a-team)
* [Fetch Team settings](#fetch-team-settings)
* [Update Team settings](#update-team-settings)
* [Fetch statistics for a Team](#fetch-statistics-for-a-team)


### List Servers for a Team

<ApiOperation method="GET" url="https://app.scaleft.com/v1/teams/${team_name}/servers" />
Lists all the Servers enrolled in a Team to which the requesting ASA User has access.

This endpoint requires one of the following roles: `reporting_user`, `server_admin`, `access_user`, `access_admin`, `authenticated_client`.

#### Request path parameters

| Parameter | Type        | Description   |
| --------- | ----------- | ------------- |
| `team_name`   | string | The name of your Team. |


#### Request query parameters

| Parameter | Type   | Description |
| --------- | ------------- | -------- |
| `alt_names_contains`   |  string | (Optional) Include Servers that contain the value of `alt_name_contains` in their `alt_names` |
| `bastion`   |  string | (Optional) A bastion hostname |
| `canonical_name`   |  string | (Optional) A canonical name |
| `cloud_provider`   |  string | (Optional) A cloud provider. Can be `aws` or `gcp` |
| `count`   |  number | (Optional) The number of objects per page. |
| `descending`   |  boolean | (Optional) The object order. |
| `hostname`   |  string | (Optional) A hostname |
| `offset`   |  string | (Optional) The page offset. |
| `prev`   |  boolean | (Optional) The direction of paging. |
| `project_name`   |  string | (Optional) A Project name |
| `state`   |  string | (Optional) State of the Server. Can be `ACTIVE` or `INACTIVE` |


#### Request body

This endpoint has no request body.

#### Response body
This endpoint returns a list of objects with the following fields and a `200` code on a successful call.
| Properties | Type        | Description          |
|----------|-------------|----------------------|
| `alt_names`   | array | (Optional) Alternative names of the Server. |
| `bastion`   | string | Specifies the bastion-host Clients will automatically use when connecting to this host. |
| `canonical_name`   | string | Specifies the name Clients should use/see when connecting to this host. Overrides the name found with hostname. |
| `cloud_provider`   | string | The cloud provider of the Server, if one exists. |
| `deleted_at`   | string | The time the Server was deleted from the Project. |
| `hostname`   | string | The hostname of the Server. |
| `id`   | string | The UUID corresponding to the Server. |
| `instance_details`   | object | Information the cloud provider provides of the Server, if one exists. |
| `last_seen`   | string | The last time the Server made a request to the ASA platform. |
| `managed`   | boolean | True if the Server is managed by 'sftd'. Unmanaged Servers are used in configurations where users may have a bastion, for example, that they don't want/can't connect to through 'sftd'. With an Unmanaged Server record to represent this box, ASA will know it exists and to use it as a bastion hop. |
| `os`   | string | The particular OS of the Server, such as CentOS 6 or Debian 9.13 |
| `os_type`   | string | The OS family the Server is running. Can be either be Linux or Windows. |
| `project_name`   | string | The Project that the Server belongs to. |
| `registered_at`   | string | The time the Server was registered to the Project. |
| `services`   | array | The service Clients use to connect to the Server. Can either be `ssh` or `rdp`. |
| `sftd_version`   | string | The version of 'sftd' the Server is running. |
| `ssh_host_keys`   | array | The host keys used to authenticate the Server. |
| `state`   | string | Can be either `ACTIVE` or `INACTIVE`. |
| `team_name`   | string | The name of the Team. |

#### Usage example

##### Request

```bash
curl -v -X GET \
-H "Authorization: Bearer ${jwt}" \
https://app.scaleft.com/v1/teams/${team_name}/servers
```

##### Response

```json
{
	"list": [
		{
			"access_address": null,
			"alt_names": null,
			"bastion": null,
			"broker_host_certs": null,
			"canonical_name": null,
			"cloud_provider": null,
			"deleted_at": "0001-01-01T00:00:00Z",
			"hostname": "harvard",
			"id": "94af8742-42f9-4560-9345-8eec11d6fded",
			"instance_details": null,
			"last_seen": "0001-01-01T00:00:00Z",
			"managed": true,
			"os": "Ubuntu 16.04",
			"os_type": "linux",
			"project_name": "the-sound-and-the-fury",
			"registered_at": "0001-01-01T00:00:00Z",
			"services": [
				"ssh"
			],
			"sftd_version": "1.44.4",
			"ssh_host_keys": null,
			"state": "INACTIVE",
			"team_name": "william-faulkner"
		},
		{
			"access_address": null,
			"alt_names": null,
			"bastion": null,
			"broker_host_certs": null,
			"canonical_name": null,
			"cloud_provider": null,
			"deleted_at": "0001-01-01T00:00:00Z",
			"hostname": "jefferson",
			"id": "be241b5e-2bc2-4c19-8300-f2d8f21fbe71",
			"instance_details": null,
			"last_seen": "0001-01-01T00:00:00Z",
			"managed": true,
			"os": "Ubuntu 16.04",
			"os_type": "linux",
			"project_name": "the-sound-and-the-fury",
			"registered_at": "0001-01-01T00:00:00Z",
			"services": [
				"ssh"
			],
			"sftd_version": "1.44.4",
			"ssh_host_keys": null,
			"state": "INACTIVE",
			"team_name": "william-faulkner"
		}
	]
}
```
### Fetch Team settings

<ApiOperation method="GET" url="https://app.scaleft.com/v1/teams/${team_name}/settings" />
Fetch Team-level settings for a specific Team, such as authentication and enrollment details.

This endpoint requires one of the following roles: `access_admin`, `instance_admin`, `access_user`.

#### Request path parameters

| Parameter | Type        | Description   |
| --------- | ----------- | ------------- |
| `team_name`   | string | The name of your Team. |


#### Request query parameters

This endpoint has no query parameters.

#### Request body

This endpoint has no request body.

#### Response body
This endpoint returns an object with the following fields and a `200` code on a successful call.
| Properties | Type        | Description          |
|----------|-------------|----------------------|
| `approve_device_without_interaction`   | boolean | If enabled, ASA will auto-approve devices for ASA Users authenticated into this Team. |
| `client_session_duration`   | number | Configure client session to be between 1 hour and 25 hours. |
| `post_device_enrollment_url`   | string | If configured, the URL a ASA User will be directed to after enrolling a device in ASA. |
| `post_login_url`   | string | If configured, this is the URL a ASA User who has not recently been authenticated will be directed to after being validated by their IDP in the course of getting credentials. |
| `post_logout_url`   | string | If configured, this is the URL a ASA User will be redirected to after logging out. |
| `reactivate_users_via_idp`   | boolean | If a disabled or deleted ASA User is able to authenticate via the IDP, their ASA user will be re-enabled. |
| `team`   | string | The name of the Team that is configured with the contained settings. |
| `user_provisioning_exact_username`   | boolean | If true, ASA will have ASA Users configured via SCIM maintain the exact username specified. |
| `web_session_duration`   | number | Configure web session to be between 30 minutes and 25 hours. |

#### Usage example

##### Request

```bash
curl -v -X GET \
-H "Authorization: Bearer ${jwt}" \
https://app.scaleft.com/v1/teams/${team_name}/settings
```

##### Response

```json
{
	"approve_device_without_interaction": false,
	"client_session_duration": 36000,
	"post_device_enrollment_url": null,
	"post_login_url": null,
	"post_logout_url": null,
	"reactivate_users_via_idp": false,
	"team": "william-faulkner",
	"user_provisioning_exact_username": null,
	"web_session_duration": 36000
}
```
### Update Team settings

<ApiOperation method="PUT" url="https://app.scaleft.com/v1/teams/${team_name}/settings" />
Update team-level settings. Partial updates are permitted. URL parameters are optional and default to unset. To unset a previously-set URL, PUT with the value set to `null`.

This endpoint requires one of the following roles: `access_admin`, `instance_admin`.

#### Request path parameters

| Parameter | Type        | Description   |
| --------- | ----------- | ------------- |
| `team_name`   | string | The name of your Team. |


#### Request query parameters

This endpoint has no query parameters.

#### Request body

This endpoint requires an object with the following fields.
| Properties | Type        | Description          |
|----------|-------------|----------------------|
| `approve_device_without_interaction`   | boolean | If enabled, ASA will auto-approve devices for ASA Users authenticated into this Team. |
| `client_session_duration`   | number | Configure client session to be between 1 hour and 25 hours. |
| `post_device_enrollment_url`   | string | If configured, the URL a ASA User will be directed to after enrolling a device in ASA. |
| `post_login_url`   | string | If configured, this is the URL a ASA User who has not recently been authenticated will be directed to after being validated by their IDP in the course of getting credentials. |
| `post_logout_url`   | string | If configured, this is the URL a ASA User will be redirected to after logging out. |
| `reactivate_users_via_idp`   | boolean | If a disabled or deleted ASA User is able to authenticate via the IDP, their ASA user will be re-enabled. |
| `team`   | string | The name of the Team that is configured with the contained settings. |
| `user_provisioning_exact_username`   | boolean | If true, ASA will have ASA Users configured via SCIM maintain the exact username specified. |
| `web_session_duration`   | number | Configure web session to be between 30 minutes and 25 hours. |

#### Response body
This endpoint returns a `204 No Content` response on a successful call.


#### Usage example

##### Request

```bash
curl -v -X PUT \
-H "Authorization: Bearer ${jwt}" \
--data '{
	"approve_device_without_interaction": false,
	"client_session_duration": 600,
	"post_device_enrollment_url": null,
	"post_login_url": null,
	"post_logout_url": null,
	"reactivate_users_via_idp": false,
	"team": "william-faulkner",
	"user_provisioning_exact_username": null,
	"web_session_duration": 600
}' \
https://app.scaleft.com/v1/teams/${team_name}/settings
```

##### Response

```json
HTTP 204 No Content
```
### Fetch statistics for a Team

<ApiOperation method="GET" url="https://app.scaleft.com/v1/teams/${team_name}/team_stats" />
Provides some general statistics regarding a Team.

This endpoint requires one of the following roles: `access_admin`.

#### Request path parameters

| Parameter | Type        | Description   |
| --------- | ----------- | ------------- |
| `team_name`   | string | The name of your Team. |


#### Request query parameters

This endpoint has no query parameters.

#### Request body

This endpoint has no request body.

#### Response body
This endpoint returns an object with the following fields and a `200` code on a successful call.
| Properties | Type        | Description          |
|----------|-------------|----------------------|
| `num_clients`   | number | The number of Clients in a Team. |
| `num_gateways`   | number | The number of Gateways in a Team. |
| `num_groups`   | number | The number of ASA Groups in a Team. |
| `num_human_users`   | number | The number of human ASA Users in a Team. |
| `num_projects`   | number | The number of Projects in a Team. |
| `num_servers`   | number | The number of Servers in a Team. |
| `num_service_users`   | number | The number of service ASA Users in a Team. |

#### Usage example

##### Request

```bash
curl -v -X GET \
-H "Authorization: Bearer ${jwt}" \
https://app.scaleft.com/v1/teams/${team_name}/team_stats
```

##### Response

```json
{
	"num_af_applications": 0,
	"num_clients": 0,
	"num_gateways": 0,
	"num_groups": 1,
	"num_human_users": 1,
	"num_oidc_applications": 0,
	"num_projects": 2,
	"num_servers": 1,
	"num_service_users": 0
}
```


