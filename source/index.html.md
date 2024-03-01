---
title: API Reference

language_tabs: # must be one of https://github.com/rouge-ruby/rouge/wiki/List-of-supported-languages-and-lexers
  - python

includes:
  - errors

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for the Kittn API
---

# Introduction

Welcome to the Beautyscan API! 

You can use our API to access Beautyscan API endpoints.

# Authentication

Будет по jwt токенам в запросах.

Coming soon...

# Model Use

## Face model use

```python
import requests
data = {
  'with_photo': True,
  'image': base64_photo,
}
requests.post(HOST+'/model/face', data=data)
```

```json
{
    "result": true,
    "data": {
      "age": 26, 
      "face_conds": {"Атрофия кожи": 0, "Расширенные поры": 15, "Купероз (телеангиэктазии)": 0, "Круги/мешки под глазами": 10, "Комедоны": 0, "Пигментация": 5, "Рубцы": 0, "Розацеа (покраснение щек)": 5, "Постакне": 10, "Папулы и пустулы при акне": 0, "Морщины": 5, "Жирный блеск": 10, "Шелушение": 5},
      "face_rank": 9.5,
      "after_face": "base64_photo",
      "before_face": "base64_photo", 
      "test_id": 12
    }
}
```

This endpoint will return face model results.

### HTTP Request

`POST /model/face`

### Query Parameters

Parameter | Required | Description
--------- |----------| -----------
image | true     | photo in base64 format
with_photo | true     | If set to true, the result will include after_face and before_face

## Nail model

### HTTP Request

`POST /model/nails`

```python
import requests
data = {
  'with_photo': True,
  'image': base64_photo,
}
requests.post(HOST+'/model/nails', data=data)
```

```json
{
  "result": true,
  "data": {
    "test_id": 112,
    "results": [
      {
       "sliced_image": "base64_image",
        "result": {}
      },
      {
        "sliced_image": "base64_image",
        "result": {}
      }
    ] 
  }
}
```

This endpoint will return nail model result for each nail find in source photo.

### URL Parameters

Parameter | Required | Description
--------- |----------| -----------
image | true     | photo in base64 format
with_photo | true     | If set to true, the result will include sliced_image


### HTTP Request

`POST /model/nail`

```python
import requests
data = {
  'with_photo': True,
  'image': base64_photo,
}
requests.post(HOST+'/model/nail', data=data)
```

```json
{
  "result": true,
  "data": {
    "sliced_image": "base64_image",
    "test_id": 112,
    "result": {}
  }
}
```

This endpoint will return nail model result for one nail find in photo.

### URL Parameters

Parameter | Required | Description
--------- |----------| -----------
image | true     | photo in base64 format
with_photo | true     | If set to true, the result will include sliced_image

# Tests

## Face create test

### HTTP Request
`POST /test/face`

```python
import requests

skin_state_codes = {
  'для всех состояний кожи': 0,
  'возрастные изменения': 1,
  'чувствительная': 2,
  'купероз': 3,
  'проблемная': 4,
  'обезвоженная': 5,
  'пигментация': 6,
  'морщины под глазами': 7
}

priority_codes = {
  'любой': 0,
  'сияние и ровный тон': 1,
  'морщины и возрастные изменения': 2,
  'снижение чувствительности и реактивности кожи': 3,
  'борьба с куперозом': 4,
  'борьба с воспалениями': 5,
  'хочу увлажненную кожу': 6,
  'борьба с расширенными порами': 7,
  'борьба с пигментацией': 8
}

skin_type_codes = {
  'для всех типов кожи': 0,
  'жирная': 1,
  'нормальная': 2,
  'комбинированная': 3,
  'сухая': 4
}

data = {
  'age': 22,
  'has_allergy': False,
  'skin_state': skin_state_codes['чувствительная'],
  'priority_codes': [priority_codes['любой'], priority_codes['сияние и ровный тон']],
  'skin_type_codes': skin_type_codes['жирная'],
  'test_id': 112
}
requests.post(HOST+'/test/face', data=data)
```

```json
{
  "result": true,
  "data": {
    "test_finished": true
  }
}
```

This endpoint will return test id when process selection will be finished.

### URL Parameters

Parameter | Required | Description
--------- |----------| -----------
age | true     | int from 1 to 120
has_allergy | true     | boolean
skin_state | true     | int from values of skin_state_codes
priority_codes | true     | list of int from values of skin_state_codes
skin_type_codes | true     | int from values of skin_state_codes


## Face test get products

### HTTP Request
`GET /test/face`


```python
import requests
requests.get(HOST+'/test/face?test_id=112')
```

```json
{
  "result": true,
  "data": {
    "products": [],
    "all_available_brands": []
  }
}
```

This endpoint will return list of products and all available brands for passed test.

### URL Parameters

Parameter | Required | Description
--------- |----------| -----------
test_id | true     | int collected from created test

# Shops

## Get shop list

### HTTP Request
`GET /shops/list`

```python
import requests
requests.get(HOST+'/shops/list')
```

```json
{
  "result": true,
  "data": {
    "shops":
      [
        {
          "location": "",
          "shop_id": 23
        },
        {
          "location": "",
          "shop_id": 24
        }
      ]
  }
}
```
This endpoint will return all available shops.

## Get shop

### HTTP Request
`GET /shops/getShop`

```python
import requests
requests.get(HOST+'/shops/getShop?shop_id=12')
```

```json
{
  "result": true,
  "data": {
    "shop":
        {
          "location": "",
          "shop_id": 12
        }
  }
}
```
This endpoint will return shop location by shop id.



# Market

## Check Basket
### HTTP Request
`GET /market/basket`

```python
import requests
requests.get(HOST+'/market/basket')
```

```json
{
  "result": true,
  "data": {
    "products": []
  } 
}
```
This endpoint will return all products in users basket.

## Add to Basket

### HTTP Request
`POST /market/basket`

```python
import requests
data = {
  'product_id': 32,
}
requests.post(HOST+'/market/basket/', data=data)
```

```json
{
  "result": true
}
```
This endpoint will add product to users basket.


## Delete from Basket

### HTTP Request
`POST /market/basket`

```python
import requests
data = {
  'product_id': 32,
}
requests.delete(HOST+'/market/basket/', data=data)
```

```json
{
  "result": true
}
```
This endpoint will delete product from users basket.

# User info

## Get user info

### HTTP Request
`GET /users/info`

```python
import requests

requests.get(HOST+'/users/info/')
```

```json
{
  "result": true,
  "data": {
    "lang_code": "en",
    "phone": "+7345324334",
    "data_last_test": "21.10.2022",
    "count_test_face": 21,
    "count_test_nail": 23,
    "last_shop_id": 12
  }
}
```

This endpoint will return info about user.

## Get user tests

### HTTP Request
`GET /users/tests/face`

```python
import requests

requests.get(HOST+'/users/tests/face?with_photo=1')
```

```json
{
  "result": true,
  "data": {
    "count": 112,
    "list_tests": [
      {
        "test_id": 123,
        "results": {
          "loaded_photo": "base64_photo",
          "photo_results": {
              "age": 26, 
              "face_conds": {"Атрофия кожи": 0, "Расширенные поры": 15, "Купероз (телеангиэктазии)": 0, "Круги/мешки под глазами": 10, "Комедоны": 0, "Пигментация": 5, "Рубцы": 0, "Розацеа (покраснение щек)": 5, "Постакне": 10, "Папулы и пустулы при акне": 0, "Морщины": 5, "Жирный блеск": 10, "Шелушение": 5},
              "face_rank": 9.5,
              "after_face": "base64_photo",
              "before_face": "base64_photo"
            },
          "form_results": {
            "products": []
          }
        }
      }
    ]
  }
}
```

### URL Parameters

Parameter | Required | Default | Description
--------- |----------| ----------- | -------
with_photo | false    | 0 | add loaded_photo and after_face, before_face to response if 1


This endpoint will return list of tests passed by users (last 100).


# Admin

## Get data for face test

### HTTP Request
`GET /admin/test/face`

```python
import requests

requests.get(HOST+'/admin/test/face?lang=en')
```


```json
{
  "result": true,
  "data": {
    "questions": 
      [
        {
          "question_text": "text text",
          "answers": [
            {
              "id": 1,
              "text": "text text"
            },
            {
              "id": 2,
              "text": "text text"
            }
          ]
        },
        {
          "question_text": "text text",
          "answers": [
            {
              "id": 21,
              "text": "text text"
            },
            {
              "id": 22,
              "text": "text text"
            }
          ]
        }
      ]
  }
}
```
This endpoint will return list of questions and available answers from db.


