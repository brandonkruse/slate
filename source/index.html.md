---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell


toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the CommentSold API!

# Authentication

> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```


> Make sure to replace `meowmeowmeow` with your API key.

CommentSold uses JWT API keys to allow access to the API. You can register a new JWT API ......

CommentSold expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`


# Products

## Get All Products


```shell
curl "https://divas.commentsold.com/api/2.0/products?app_version=1.3&category=1&skip=0"
  -H "Authorization: meowmeowmeow"
```


> The above command returns JSON structured like this:

```json
[
  {
    "prod_id":4096,
    "post_id":7343,
    "category":"shirt",
    "created_time":1504116005,
    "prod_name":"Varsity stripes sleeves top",
    "description":"Hey Divas!! Be super comfortable in style, when you wear this loose fit top with athletic sleeves. Pair this fun top with your choice of jeans or leggings, to be ready for wherever your day takes you. Order now!!",
    "measurements":"Medium Measurements: Bust- 50\"\/ Length- 28\"\r\nMaterials: 95% Viscose \/ 5% Spandex\r\nBrand: Eesome",
    "qty":null,
    "base_retail":"24",
    "style":"sku7157",
    "filename":"598893d453788.jpg",
    "likes":0,
    "inventory":
    [
      {
        "inventory_id":30542,
        "qty":0,
        "color":"rust",
        "size":"large"
      },      
      {
        "inventory_id":30537,
        "qty":3,
        "color":"blue",
        "size":"small"
      }
    ],

  }
]
```

This endpoint retrieves all products.

### HTTP Request

`GET /api/2.0/products`

### Query Parameters

Parameter | Description
--------- | -------
app_version |
category |
skip |

<aside class="notice">
Authentication is not required
</aside>

## Get a Specific product

```shell
curl "https://divas.commentsold.com/api/2.0/products?app_version=1.3/1400"
  -H "Authorization: meowmeowmeow"
```

This endpoint retrieves a specific product.

<aside class="notice">Important to ignore cache data.</aside>

### HTTP Request

`GET /api/2.0/products/<ID>`

Parameter | Description
--------- | -------
app_version |


## Get a Waitlist Products

```shell
curl "https://divas.commentsold.com/api/2.0/products/waitlist?app_version=1.3"
  -H "Authorization: meowmeowmeow"
```

This endpoint retrieves a waitlist products.

### HTTP Request

`GET /api/2.0/products/waitlist`

Parameter | Description
--------- | -------
app_version |

## Get Search Results

```shell
curl "https://divas.commentsold.com/api/2.0/products/search?app_version=1.3&size=5&category=dress&instock=true&skip=10"
  -H "Authorization: meowmeowmeow"
```

This endpoint retrieves a waitlist products.

### HTTP Request

`GET /api/2.0/products/search?`

Parameter | Description
--------- | -------
app_version |
size |
category |
instock |
skip |



# Cart

## Get Cart

```shell
curl "https://divas.commentsold.com/api/2.0/cart?app_version=1.3"
  -H "Authorization: meowmeowmeow"
```

### HTTP Request

`GET /api/2.0/cart`

Parameter | Description
--------- | -------
app_version |

## Get Past Orders

```shell
curl "https://divas.commentsold.com/api/2.0/carts?app_version=1.3"
  -H "Authorization: meowmeowmeow"
```


### HTTP Request

`GET /api/2.0/carts`

Parameter | Description
--------- | -------
app_version |

## Add Item

```shell
curl "https://divas.commentsold.com/api/2.0/cart/products"
  -H "Authorization: meowmeowmeow"
```

### HTTP Request

`POST /api/2.0/cart/products`

Parameter | Description
--------- | -------
app_version |
id |
inventory_id |


## Delete Item


### HTTP Request

`DELETE /api/2.0/cart/products/<inventory_id>`

Parameter | Description
--------- | -------
app_version |

## Add Coupon


### HTTP Request

`POST /api/2.0/cart/coupon`

Parameter | Description
--------- | -------
app_version |
code |

# Stripe

## Get All Cards


### HTTP Request

`GET /api/2.0/stripe/cards`

Parameter | Description
--------- | -------
app_version |

## Add Card


### HTTP Request

`POST /api/2.0/stripe/cards`

Parameter | Description
--------- | -------
app_version |
id |

## Delete Card


### HTTP Request

`DELETE /api/2.0/stripe/cards/<ID>`

Parameter | Description
--------- | -------
app_version |

## Set Default Card


### HTTP Request

`PUT /api/2.0/stripe/cards/<ID>`

Parameter | Description
--------- | -------
app_version |

# Checkout

## Stripe



### HTTP Request

`POST /api/2.0/checkout/stripe`

Parameter | Description
--------- | -------
app_version |
oid |
amount |
token |

## PayPal


### HTTP Request

`POST /api/2.0/checkout/paypal`

Parameter | Description
--------- | -------
app_version |
oid |
paymentjson |

## Get Balance


### HTTP Request

`GET /api/2.0/checkout/balance`

Parameter | Description
--------- | -------
app_version |

## Use Balance



### HTTP Request

`POST /api/2.0/checkout/balance`

Parameter | Description
--------- | -------
app_version |
oid |

# Settings

## Set Address



### HTTP Request

`POST /api/2.0/address`

Parameter | Description
--------- | -------
app_version |
street |
apt |
state |
city |
zip |

## Get Address


### HTTP Request

`GET /api/2.0/address`

Parameter | Description
--------- | -------
app_version |

## Update Email


### HTTP Request

`PUT /api/2.0/email`

Parameter | Description
--------- | -------
app_version |
email |


## Get Email


### HTTP Request

`GET /api/2.0/email`

Parameter | Description
--------- | -------
app_version |
