# Seia-Soto/maskd-releases

The Node.JS based API server that provides data of nCov-19 of South Korea from the spread sources.

> After version `9140a4a8249e75e15add00454742aae28375c3c8`, the docs will be provided in English.

## Table of Contents

- [Documentation](#documentation)
  - [Entrypoint](#entrypoint)
  - [Ratelimit](#ratelimit)
  - [Limitations](#limitations)
  - [Routes](#routes)

----

# Documentation

## Entrypoint

The entrypoint of this API is following format: `api-v{version}.maskd.seia.io`

| version | entrypoint |
| :------------- | :------------- |
| v0 | api-v0.maskd.seia.io |

## Ratelimit

This API adopted following rate-limiting policy: `8r/s up to 32 burst requests per IP address`

## Limitations

### Request

- This API doesn't support `multipart` form data.

### Delay

- The data of the mask stock status have 5~10 mins of delay.

## Routes

### POST: /clinics/selection

Get the list of available selection clinics.

#### Request form

- `scope`: The key name of values such as `identify`, `city`, ... (default: `clinicName`)
- `keyword`: The string need to be used to search items. If you don't send this parameter, the all items will be printed. (default: `none`)

#### Response format

```json
[
  {
   "id": 3,
   "identify": 3,
   "samplingAvailable": 1,
   "province": "서울",
   "city": "강남구",
   "clinicName": "연세대학교의과대학강남세브란스병원",
   "address": "서울특별시 강남구 언주로 211, 강남세브란스병원 (도곡동)",
   "representativeContact": "02-2019-3114"
  }
]
```

- `id`: The identification tag for internal use.
- `identify`: The national identification code.

### POST: /clinics/safelySeparated

Get the list of available safely separated clinics.

#### Request form

- `scope`: The key name of values such as `identify`, `city`, ... (default: `clinicName`)
- `keyword`: The string need to be used to search items. If you don't send this parameter, the all items will be printed. (default: `none`)

#### Response format

```json
[
  {
    "id": 1,
    "identify": 1,
    "province": "서울",
    "city": "강남구",
    "clinicName": "연세대학교강남세브란스병원",
    "address": "서울특별시 강남구 언주로211",
    "requestType": "외래진료 및 입원",
    "representativeContact": "02-2019-2114",
    "availableAt": "2020-02-28T00:00:00.000Z"
  }
]
```

- `id`: The identification tag for internal use.
- `identify`: The national identification code.
- `availableAt`: Datetime in KST.

### POST: /masks/stores

Get the list of available stores that sell masks.

#### Request form

- `limit`: The number of items you want to get at the time. (default: `250`, max: `1000`)
- `page`: The number that express the `n`th part of the result if the size of the result is bigger than `limit` parameter. (default: `1`)
- `scope`: The key name of values such as `identify`, `city`, ... (default: `clinicName`)
- `keyword`: The string need to be used to search items. This value should be 2 words at least. (default: `none`)

#### Response form

```json
[
  {
    "id": 8,
    "identify": 11889446,
    "address": "서울특별시 강남구 압구정로 108 1층 7호 (신사동)",
    "latitude": "37.5228904",
    "longitude": "127.0206138",
    "name": "가로수길약국",
    "type": 1,
    "stockReplenishedAt": null,
    "stockStatus": "unavailable",
    "stockUpdatedAt": null,
    "updatedAt": "2020-03-15T14:19:38.000Z"
  }
]
```

- `id`: The identification tag for internal use.
- `identify`: The national identification code.
- type
  - `1`: pharmacy
  - `2`: post office
  - `3`: NH
- stockStatus (the number of the masks available)
  - `plenty`: 100+,
  - `some`: 30+,
  - `few`: 2+,
  - `empty`: 1 or lower,
  - `break`: stopped selling
- `stockUpdatedAt`: (KST) The last time that checked the status of the mask stock in each store.
- `updatedAt`: (UTC) The last time updated the data from official source.
