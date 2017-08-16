---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='https://www.memcachier.com'>Sign Up for a Memcachier Account</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

search: true
---

# Introduction

The purpose of this API is to give you access to some of the features available on the Memcachier Analytics Dashboard. Our analytics dashboard is a simple tool that gives you more insight into how you’re using memcache.

To access your application's analytics dashboard login to your account and view one of your caches.

The analytics API allows you to

- [Flush](#flush) your cache.

- Collect [current](#stats) as well as [historical](#history) statistical information about your cache usage.

- [Get](#get-a-list-of-credentials), [add](#create-a-new-set-of-credentials), [update](#update-a-set-of-credentials), [promote](#promote-a-set-of-credentials), and/or [delete](#delete-a-set-of-credentials) a set of credentials connected to your cache.

# Authentication

> To authorize, use this code:

```shell
curl "https://analytics.memcachier.com/api/v1/:memcachier_id/:action"
  --user CRED_USERNAME:CRED_PASSWORD
```

> Make sure to replace `CRED_USERNAME:CRED_PASSWORD` with your credential username and password found on the analytics dashboard.

Memcachier uses credentials to allow access to the API. After you've created a cache, you can find your credentials on the [analytics dashboard](https://analytics.memcachier.com/). Only credentials that have the API capability will be allowed to use this API.

Memcachier expects for your credentials to be included in the header of all API requests.

# Memcachier ID

```shell
curl "https://analytics.memcachier.com/api/v1/login"
  --user CRED_USERNAME:CRED_PASSWORD
```

> The above command will return your memcachier ID:

```json
{
    "cache_id": 15
}
```

All of the API paths include a `<memcachier_id>` variable. In order to find this number, you'll need to use the /login path.

### HTTP Request

`GET https://analytics.memcachier.com/api/login`

### Responses

Status| Response
------|-----------
200   | JSON response with memcachier id
403   | "You are not authorized to perform this action."
404   | "No cache found."
500   | "Server error"

<aside class="notice">
This is not the same thing as the "Memcachier ID" listed on your analytics dashboard.
</aside>

# Stats

## Get All Statistics for a Cache

```shell
curl "https://analytics.memcachier.com/api/v1/<memcachier_id>/stats"
  --user "CRED_USERNAME:CRED_PASSWORD"
```

> The above command returns JSON structured like this:

```json
{
    "a.b.c.totallyaserver.com:12345": {
        "auth_cmds": 19,
        "auth_errors": 0,
        "bytes": 0,
        "bytes_read": 960,
        "bytes_written": 22233,
        "cas_badval": 0,
        "cas_hits": 0,
        "cas_misses": 0,
        "cmd_delete": 0,
        "cmd_flush": 0,
        "cmd_get": 0,
        "cmd_set": 0,
        "cmd_touch": 0,
        "curr_connections": 0,
        "curr_items": 0,
        "decr_hits": 0,
        "decr_misses": 0,
        "delete_hits": 0,
        "delete_misses": 0,
        "evictions": 0,
        "expired": 0,
        "get_hits": 0,
        "get_misses": 0,
        "incr_hits": 0,
        "incr_misses": 0,
        "limit_maxbytes": 28835840,
        "time": 1500651085,
        "total_connections": 19,
        "total_items": 0,
        "touch_hits": 0,
        "touch_misses": 0
    }
}
```

This endpoint retrieves all the statistics for your cache.

### HTTP Request

`GET https://analytics.memcachier.com/api/v1/<memcachier_id>/stats`


### Responses

Status| Response
------|-----------
200   | JSON response with stats
403   | "You are not authorized to perform this action."
500   | "Server error: a.b.c.totallyaserver.com:1234,..."

# History

## Get All Statistical History for a Cache

```shell
curl "https://analytics.memcachier.com/api/v1/<memcachier_id>/history"
  --user "CRED_USERNAME:CRED_PASSWORD"
```

> The above command returns JSON structured like this:

```json
  [{
    "memcachier_id": "11",
    "server": "a.b.c.totallyaserver.memcachier.com",
    "stats": {
        "auth_cmds": 158,
        "auth_errors": 0,
        "bytes": 0,
        "bytes_read": 3840,
        "bytes_written": 178754,
        "cas_badval": 0,
        "cas_hits": 0,
        "cas_misses": 0,
        "cmd_delete": 0,
        "cmd_flush": 0,
        "cmd_get": 0,
        "cmd_set": 0,
        "cmd_touch": 0,
        "curr_connections": 2,
        "curr_items": 0,
        "decr_hits": 0,
        "decr_misses": 0,
        "delete_hits": 0,
        "delete_misses": 0,
        "evictions": 0,
        "expired": 0,
        "get_hits": 0,
        "get_misses": 0,
        "incr_hits": 0,
        "incr_misses": 0,
        "limit_maxbytes": 1153433600,
        "time": 1490731542,
        "total_connections": 158,
        "total_items": 0,
        "touch_hits": 0,
        "touch_misses": 0
    },
    "timestamp": "1490731541096"
    }, … ]

```

This endpoint retrieves the statistical history of a cache.

### HTTP Request

`GET https://analytics.memcachier.com/api/v1/<memcachier_id>/history`

### Responses

Status| Response
------|-----------
200   | JSON response with stats
403   | "You are not authorized to perform this action."
500   | "Server error"

# Flush

## Flush All the Data from the Cache

```shell
curl "https://analytics.memcachier.com/api/v1/<memcachier_id>/flush" -X POST
  --user "CRED_USERNAME:CRED_PASSWORD"
```

> The above command will return a 200 success message of successful.


This endpoint will flush all of the data from the cache cache.

<aside class='notice'>Certain credentials may not have permission to flush the cache, which will produce a 403 error.</aside>

### HTTP Request

`POST https://analytics.memcachier.com/api/v1/<memcachier_id>/flush`

### Responses

Status| Response
------|-----------
200   | ""
403   | "You are not authorized to perform this action."
500   | "Server error: a.b.c.totallyaserver.com:1234,..."

# Credentials

You can read more about what credentials are and how to use them in our [platform documentation](https://www.memcachier.com/documentation#credentials).

## Get A List of Credentials

```shell
curl "https://analytics.memcachier.com/api/v1/<memcachier_id>/credentials"
  --user "CRED_USERNAME:CRED_PASSWORD"
```

> The above command returns JSON structured like this:

```json
  [
    {
        "cache_id": 14,
        "flush_capability": false,
        "id": 44,
        "sasl_username": "123456",
        "uuid": null,
        "write_capability": true,
        "api_capability": true,
        "primary": true,
    },
    {
        "cache_id": 14,
        "flush_capability": false,
        "id": 43,
        "sasl_username": "789101",
        "uuid": null,
        "write_capability": true,
        "api_capability": true,
        "primary": false,
    }, ...
]

```

The endpoint returns a list of all the credentials connected to the cache.

### HTTP Request

`GET https://analytics.memcachier.com/api/v1/<memcachier_id>/credentials`

### Responses

Status| Response
------|-----------
200   | JSON list of credentials
403   | "You are not authorized to perform this action."
500   | "Server error"

## Create a New Set of Credentials

```shell
curl "https://analytics.memcachier.com/api/v1/<memcachier_id>/credentials" -X POST
  --user "CRED_USERNAME:CRED_PASSWORD"
```

> The above command returns JSON structured like this:

```json
{
    "cache_id": 14,
    "id": null,
    "sasl_password": "EE39F0F3D0C11330451F0B148BC7C24E",
    "sasl_username": "51B791",
    "uuid": null,
    "primary": false,
    "flush_capability": true,
    "write_capability": true,
    "api_capability": true,
}

```
This endpoint creates a new set of credentials which can be used to connect to the cache.

### HTTP Request

`POST https://analytics.memcachier.com/api/v1/<memcachier_id>/credentials`

### Responses

Status| Response
------|-----------
200   | JSON of new credential properties
403   | "You are not authorized to perform this action."
500   | "Server error"

## Update a Set of Credentials

```shell
curl "https://analytics.memcachier.com/api/v1/<memcachier_id>/credentials/<cred_username>" -X PATCH
  -d '{"flush_capability":"false"}'
  --user "CRED_USERNAME:CRED_PASSWORD"
```

> The above command returns JSON structured like this:

```json
{
    "flush_capability": false,
    "write_capability": true,
    "api_capability": true,
}

```
This endpoint updates the capabilities of a specific set of credentials.

### HTTP Request

`POST https://analytics.memcachier.com/api/v1/<memcachier_id>/credentials/<cred_id>`

### Query Parameters

Parameter        | Default | Description
-----------------|---------|-------------
flush_capability | true    | Authorize this set of credentials to flush the cache.
write_capability | true    | Authorize this set of credentials to write to the cache.
api_capability   | true    | Authorize this set of credentials to use this API.

### Responses

Status| Response
------|-----------
200   | JSON of new credential properties
403   | "You are not authorized to perform this action."
500   | "Server error"

## Promote a Set of Credentials

```shell
curl "https://analytics.memcachier.com/api/v1/<memcachier_id>/credentials/primary/<cred_username>" -X POST
  --user "CRED_USERNAME:CRED_PASSWORD"
```

> The above command returns JSON structured like this:

```json
{
    "cache_id": 14,
    "id": null,
    "sasl_username": "51B791",
    "uuid": null,
    "primary": true,
    "flush_capability": false,
    "write_capability": true,
    "api_capability": true,
}

```

### HTTP Request

`POST https://analytics.memcachier.com/api/v1/<memcachier_id>/credentials/primary/<cred_id>`

### Responses

Status| Response
------|-----------
200   | JSON of new credential properties
403   | "You are not authorized to perform this action."
500   | "Server error"

## Delete a Set of Credentials

```shell
curl "https://analytics.memcachier.com/api/v1/<memcachier_id>/credentials/primary/<cred_username>" -X DELETE
  --user "CRED_USERNAME:CRED_PASSWORD"
```

> The above command returns a 200 status code if successful.

### HTTP Request

`POST https://analytics.memcachier.com/api/v1/<memcachier_id>/credentials/primary/<cred_id>`

### Responses

Status| Response
------|-----------
200   | ""
403   | "You are not authorized to perform this action."
500   | "Server error"

