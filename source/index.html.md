---
title: Kirin API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors
---

# Introduction
This project is just for Liulishuo interview, I'm drinking Kirin beer when I create the code repository that's why it is named as Kirin(麒麟).

# Authentication

> To authorize, use this code:

```shell
# With shell, you can just pass the correct JWT in Authorization header with each request
curl -H "Authorization: Bearer <JWT>" \
     "api_endpoint_here"
```

Authenticate failed:

- 401, no JWT provided or invalid JWT;
- 403, valid JWT but have no privilege to access the resource;

> Make sure to replace `<JWT>` with your correct JWT.

## Get the JWT
Get your JWT by post `user_token` endpoint with email and password.

```shell
curl -XPOST -H "Content-Type: application/json" \
     -d '{ "auth": { "email": "holysoros@gmail.com", "password": "password" } }' \
     http://kirin.julewu.com/user_token
```

> JSON response

```json
{
    "jwt": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE1MDU1Njg5NjgsInN1YiI6MX0.2Os_C9zyVgo87u9zqUwcFvim8nm0H3ne56P8gJuajmk"
}
```

Kirin expects for the JWT key to be included in all API requests to the server in a header that looks like the following:

`Authorization: Bearer <JWT>`

<aside class="notice">
You must replace <code>JWT</code> with your personal JWT key.
</aside>

# Red Packet(口令红包)

## Create a red packet

```shell
curl -XPOST -H "Content-Type: application/json" \
     -H "Authorization: Bearer <JWT>" \
     -d '{ "amount": 10, "packet_number": 3, "greeting": "Best wishes" }' \
     http://kirin.julewu.com/red_packets
```

> The above command returns JSON structured like this:

```json
{
  "id": 13,
  "number": "201709151432031646553377299",
  "amount": "101.0",
  "packet_number": 3,
  "greeting": null,
  "code": "Ek1uQJ7T",
  "created_at": "2017-09-15T14:32:03.166Z",
  "lucky_draws": [],
  "sender": {
    "id": 1,
    "name": "BinLi",
    "email": "holysoros@gmail.com"
  }
}
```

## Open the red packet with code
Open the red packet is just to create a lucky draw for a red packet.

Return 201 if success to open the red packet, and return 400 if failed. A message with given to specify why the request failed:

- Incorrect code
- The red packet is expired or finished
- You have opened the red packet

```shell
curl -XPOST -H "Content-Type: application/json" \
     -H "Authorization: Bearer <JWT>" \
     -d '{ "code": "DVfiI86B" }' \
     http://kirin.julewu.com/red_packets/8/lucky_draws
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
    "amount": "1.15",
    "created_at": "2017-09-15T14:33:15.425Z",
    "red_packet": {
      "id": 13,
      "number": "201709151432031646553377299",
      "amount": "101.0",
      "packet_number": 3,
      "greeting": null,
      "code": "Ek1uQJ7T",
      "created_at": "2017-09-15T14:32:03.166Z"
    },
    "user": {
      "id": 1,
      "name": "BinLi",
      "email": "holysoros@gmail.com"
    }
}
```

Return 400 status code if some error happened.

```json
{"message":"Incorrect code"}
```

## Get all red packet you sent

```shell
curl -H "Accept: application/json" \
     -H "Authorization: Bearer <JWT>" \
     http://kirin.julewu.com/red_packets
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 4,
      "number": "201709150925412540568238214",
      "amount": "199.0",
      "packet_number": 4,
      "greeting": "Hello world",
      "code": "Ny288dyH",
      "created_at": "2017-09-15T09:25:41.256Z",
      "lucky_draws": [],
      "sender": {
        "id": 1,
        "name": "BinLi",
        "email": "holysoros@gmail.com"
      }
  }
]
```

This endpoint retrieves all red packets you sent.

## Get a specific red packet you sent

```shell
curl -H "Accept: application/json" \
     -H "Authorization: Bearer <JWT>"
     http://kirin.julewu.com/red_packets/8
```

# LuckyDraw(红包抽奖)

## Get all lucky draws you got
Open a red packet to get a lucky draw.

```shell
curl -H "Accept: application/json" \
     -H "Authorization: Bearer <JWT>" \
     http://kirin.julewu.com/lucky_draws
```

> The above command returns JSON structured like this:

```json
[
  {
    "id":1,
    "amount":"1.15",
    "created_at":"2017-09-15T14:00:50.767Z",
    "red_packet": {
      "id":8,
      "number":"201709151400357642349271522",
      "amount":"10.0",
      "packet_number":3,
      "greeting":"Best wishes",
      "code":"DVfiI86B",
      "created_at":"2017-09-15T14:00:35.765Z"
    }
  }
]
```

## Get a specific lucky draw you got

```shell
curl -H "Accept: application/json" \
     -H "Authorization: Bearer <JWT>"
     http://kirin.julewu.com/lucky_draws/1
```

This endpoint retrieves a specific lucky draw.


## Get wallet detail
Get my balance and red packets list etc.

```shell
curl -H "Accept: application/json" \
     -H "Authorization: Bearer <JWT>" \
     http://kirin.julewu.com/wallet
```

> The above command returns JSON structured like this:

```json
{
    "balance": "101.1",
    "lucky_draws": [
        {
            "id": 1,
            "amount": "1.15",
            "created_at": "2017-09-15T14:00:50.767Z",
            "red_packet": {
                "id": 8,
                "number": "201709151400357642349271522",
                "amount": "10.0",
                "packet_number": 3,
                "greeting": "Best wishes",
                "code": "DVfiI86B",
                "created_at": "2017-09-15T14:00:35.765Z"
            }
        }
    ],
    "red_packets": [
        {
            "id": 7,
            "number": "201709151352040424570439248",
            "amount": "10.0",
            "packet_number": 3,
            "greeting": "Best wishes",
            "code": "7GvuD6gR",
            "created_at": "2017-09-15T13:52:04.044Z",
            "lucky_draws": []
        },
        {
            "id": 8,
            "number": "201709151400357642349271522",
            "amount": "10.0",
            "packet_number": 3,
            "greeting": "Best wishes",
            "code": "DVfiI86B",
            "created_at": "2017-09-15T14:00:35.765Z",
            "lucky_draws": [
                {
                    "id": 1,
                    "amount": "1.15",
                    "created_at": "2017-09-15T14:00:50.767Z"
                }
            ]
        }
    ]
}
```
