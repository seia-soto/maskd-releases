# Seia-Soto/maskd-release

`Seia-Soto/maskd`에 대한 한국어 문서입니다.

## 목차

- [Documentation](#documentation)
  - [Entrypoint](#entrypoint)
  - [Ratelimit](#ratelimit)
  - [Limitations](#limitations)
  - [Routes](#routes)

----

# Documentation

## Entrypoint

이 API의 엔트리 포인트는 다음의 형식을 따르고 있습니다: `api-v{버전}.maskd.seia.io`

| 버전 | 엔트리 포인트 |
| :------------- | :------------- |
| v0 | api-v0.maskd.seia.io |

## Ratelimit

이 API는 다음의 Rate-limiting 규칙을 가지고 있습니다: `8r/s up to 32 burst requests per IP address`

## Limitations

- Request
  - 이 API는 `multipart` 폼 데이터를 해석할 수 없습니다.

## Routes

### POST: /clinics/selection

선별진료소의 목록을 불러옵니다.

#### Request form

- `scope`: `identify`, `city`, ...와 같은 항목의 이름입니다. (기본값: `clinicName`)
- `keyword`: 항목을 검색하는데에 쓰이는 문자열입니다. 이 파라메터를 전송하지 않으면 모든 항목이 출력됩니다. (기본값: `없음`)

#### Response format

```json
[
  {
    "identify": 2,
    "samplingAvailable": 1,
    "province": "서울",
    "city": "강남구",
    "clinicName": "삼성서울병원",
    "address": "서울특별시 강남구 일원로 81 (일원동, 삼성의료원)",
    "representativeContact": "02-3410-2114"
  }
]
```

### POST: /clinics/safelySeparated

국민 안심 병원의 목록을 불러옵니다.

#### Request form

- `scope`: `identify`, `city`, ...와 같은 항목의 이름입니다. (기본값: `clinicName`)
- `keyword`: 항목을 검색하는데에 쓰이는 문자열입니다. 이 파라메터를 전송하지 않으면 모든 항목이 출력됩니다. (기본값: `없음`)

#### Response format

```json
[
  {
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

### POST: /masks/stores

공적 마스크 판매점의 목록을 불러옵니다.

#### Request form

- `scope`: `identify`, `city`, ...와 같은 항목의 이름입니다. (기본값: `clinicName`)
- `keyword`: 항목을 검색하는데에 쓰이는 문자열입니다. 최소한 2자를 필요로 합니다. (기본값: `없음`)

#### Response format

```json
[
  {
    "identify": 12808776,
    "address": "서울특별시 중구 다산로 72 1층 (신당동, 서울시니어스타워)",
    "latitude": 38,
    "longitude": 127,
    "name": "서울시니어스약국",
    "type": 1,
    "stockReplenishedAt": "2020-03-15T08:54:00.000Z",
    "stockStatus": "plenty",
    "stockUpdatedAt": "2020-03-15T12:30:00.000Z",
    "updatedAt": null
  }
]
```

- type
  - `1`: 약국
  - `2`: 우체국
  - `3`: 농협

- stockStatus (판매 가능한 마스크의 수)
  - `plenty`: 100+,
  - `some`: 30+,
  - `few`: 2+,
  - `empty`: 1개 혹은 없음,
  - `break`: 판매 중단
