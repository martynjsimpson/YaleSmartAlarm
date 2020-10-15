# Introduction
This README is to provide some documentation on the undocumented API for Yale Sync Smart Alarm.

I am in no way afiliated with Yale.

# Notice

The API's are likely subject to change at any time.

# Attribution

This work is a reverse engineering of the Python Script provided by jonathan fielding here - https://github.com/jonathan-fielding/yalealarmsystem

# Overview

The Yale Sync Smart Alarm is the newer generation of Yale Home Alarm which integrates with Alexa and Google Home.

The API EndPoints are "public", you are communicating with a Cloud service which in turn sends messages to your Yale Hub.

The Get and Set endpoints requre an access_token which is retrieved by calling the Auth endpoint.

# EndPoints

## Authentiation

The authenitcation endpoint is used to retrieve the access_token required for all subsequent requests.

From what I can gather, the value provided in the "Authorization" header is fixed and does not change from user to user.

The data can either be sent in the URL as parameters (not reccomended) or in the body as form data.

### Request
```JSON
POST https://mob.yalehomesystem.co.uk/yapi/o/token/

Headers:
Authorization: "Basic VnVWWDZYVjlXSUNzVHJhcUVpdVNCUHBwZ3ZPakxUeXNsRU1LUHBjdTpkd3RPbE15WEtENUJ5ZW1GWHV0am55eGhrc0U3V0ZFY2p0dFcyOXRaSWNuWHlSWHFsWVBEZ1BSZE1xczF4R3VwVTlxa1o4UE5ubGlQanY5Z2hBZFFtMHpsM0h4V3dlS0ZBcGZzakpMcW1GMm1HR1lXRlpad01MRkw3MGR0bmNndQ"
Content-Type: multipart/form-data; boundary={length}

Body:
grant_type=password
username:{username}
password:{password}
```

### Response

```JSON
{
    "scope": "groups basic_profile write google_profile read",
    "token_type": "Bearer",
    "expires_in": 259200,
    "access_token": "{access_token}",
    "refresh_token": "{refresh_token}"
}
```

Use the value provided in "access_token" in the Authorization header for all subsequent requests.

## Get Status

Used to get the status of your system.

### Request

```JSON
GET https://mob.yalehomesystem.co.uk/yapi/api/panel/mode/

Headers:
Authorization: Bearer {access_token}
```

### Response

```JSON
{
    "result": true,
    "code": "000",
    "message": "OK!",
    "token": "{request_token}",
    "data": [
        {
            "area": "1",
            "mode": "disarm"
        }
    ],
    "time": "0.0029"
}
```

| Field     | Description                 |
| --------- | --------------------------- |
| Result    | boolean                     |
| code      | string (unknown)            |
| message   | string                      |
| token     | string (unknown)            |
| data      | array                       |
| data.area | string (usually 1)          |
| data.mode | string (disarm/arm/home)    |
| time      | string (time for execution) |

