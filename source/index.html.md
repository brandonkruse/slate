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

# Base Shop

Each CommentSold API request is based on a particular 'shop' - for example: https://commentsold.com/divas/api/2.0/status

Since CommentSold's server infrastructure dynamically caches content and uses CDNs to sheild the origins, all requests should route through the CDN - for example: https://cdn.commentsold.com/divas/api/2.0/status

It is recommended that you use your JWT with each request, though it will be stripped automatically on non-account-specific requests

# Authentication

> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "https://cdn.commentsold.com//shop/api/2.0/status"
  -H "Authorization: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ"
```

CommentSold uses JWT API keys to allow access to the API.

![JWT diagram](/images/jwt-diagram.png)

CommentSold expects for the JWT to be included in all API requests to the server in a header that looks like the following:

`Authorization: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ`

CommentSold also expects that the app_version is set in all API requests as a GET/POST parameter:

```shell
curl "https://cdn.commentsold.com/divas/api/2.0/status?app_version=1.4
```

<aside class="notice">
Note: If the JWT is invalid/expired/etc - we will 403 requests which should prompt re-authentication
</aside>

## Sign in

```shell
curl "https://cdn.commentsold.com/divas/api/2.0/signin"
```

> The above command returns JSON structured like this:

```json
{
  "jwt": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ",
  "email": "brandon@commentsold.com",
  "freeShipping": 400,
  "vip": false
}
```
Notice that we will return other user information - email is the user's email (if available), freeShipping is the amount of time (in seconds) of free shipping. 0 seconds for no free shipping. VIP is a boolean to access certain feeds

To sign in you should use Facebook accessToken and userID.

### HTTP Request

`POST /shop/api/2.0/signin`

Parameter | Description
--------- | -------
app_version | The app_version that is built-in with the app launch. This should correspond to a specific release of the app
accessToken | The accessToken from Facebook authenticating the login
userID | The userID of the user logging in (how does the app have this? XXX)


## Sign out


```shell
# With shell, you can just pass the correct header with each request
curl "https://cdn.commentsold.com/divas/api/2.0/signout"
  -H "Authorization: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ"
```

To sign out you should explicitly invalidate token

### HTTP Request

`POST /shop/api/2.0/signout`

Parameter | Description
--------- | -------
app_version | The app_version that is built-in with the app launch. This should correspond to a specific release of the app

# Products

## Get the Product Feed according to the feed of how the customer posted them/made them available


```shell
curl "https://cdn.commentsold.com/divas/api/2.0/products/feed?app_version=1.3&feed_type=1&skip=0"
  -H "Authorization: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ"
```


> The above command returns JSON structured like this:

```json
[
  {
    "prod_id":4096,
    "post_id":7343,
    "feed_type":"shirt",
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

This endpoint retrieves a feed of products chronologically based on how the CommentSold customer posts them to Facebook

### HTTP Request

`GET /shop/api/2.0/products/feed`

### Query Parameters

Parameter | Description
--------- | -------
app_version | The app_version that is built-in with the app launch. This should correspond to a specific release of the app
feed_type | If the user has multiple feeds they post to (for example Divas, Kids, Diva VIP), then you specify them here. Each feed type has a numeric assigned to it (1, 2, 3 respectively)
skip | Default is 0 - use this parameter to acheive pagination. You should deduplicate results based on the post_id as the primary key
limit (optional) | Default is 10 

<aside class="notice">
Authentication is not required for this request
</aside>

## Get a Specific product

```shell
curl "https://cdn.commentsold.com/divas/api/2.0/products/id/1400?app_version=1.3"
  -H "Authorization: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ"
```

> The above curl returns something similar to this:

```json
[ 
  { 
    "prod_id":4096,
    "feed_type":"shirt",
    "created_time":1504116005,
    "prod_name":"Varsity stripes sleeves top",
    "description":"Hey Divas!! Be super comfortable in style, when you wear this loose fit top with athletic sleeves. Pair this fun top with your choice of jeans or leggings, to be ready for wherever your day takes you. Order now!!",
    "measurements":"Medium Measurements: Bust- 50\"\/ Length- 28\"\r\nMaterials: 95% Viscose \/ 5% Spandex\r\nBrand: Eesome",
    "qty":null,
    "base_retail":"24",
    "style":"sku7157",
    "filename":"https://cdn.commentsold.com/shop/public/images/598893d453788.jpg",
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

This endpoint retrieves details of a specific product. Note that filename will be the full URL of the image so we can dynamically switch between CDN loading, S3 and direct server depending on the customer 

<aside class="notice">Important to ignore any client-side cached data. You should diff the JSON to see changes</aside>

### HTTP Request

`GET /shop/api/2.0/products/<ID>`

Parameter | Description
--------- | -------
app_version | The app_version that is built-in with the app launch. This should correspond to a specific release of the app

> Returns JSON similar to this:

```json
[
  {
    "prod_id":4096,
    "feed_type":"shirt",
    "created_time":1504116005,
    "prod_name":"Varsity stripes sleeves top",
    "description":"Hey Divas!! Be super comfortable in style, when you wear this loose fit top with athletic sleeves. Pair this fun top with your choice of jeans or leggings, to be ready for wherever your day takes you. Order now!!",
    "measurements":"Medium Measurements: Bust- 50\"\/ Length- 28\"\r\nMaterials: 95% Viscose \/ 5% Spandex\r\nBrand: Eesome",
    "qty":null,
    "base_retail":"24",
    "style":"sku7157",
    "filename":"https://cdn.commentsold.com/shop/public/images/598893d453788.jpg",
    "likes":0,
    "comments":
    [
      {
	"comment_id": 1234,
	"comment_text": "Sold large",
	"user_photo": "https://facebook.com/whatever.jpg"
     }
    ],
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


## Get Search Results

```shell
curl "https://cdn.commentsold.com/divas/api/2.0/products/search?app_version=1.3&size=Small&feed_type=dress&instock=true&skip=10"
  -H "Authorization: meowmeowmeow"
```

This endpoint retrieves products based on custom input filters. You should dedup the prod_id in the response.

### HTTP Request

`GET /shop/api/2.0/products/search`

Parameter | Description
--------- | -------
app_version | The app_version that is built-in with the app launch. This should correspond to a specific release of the app
size | By default, can be one or more comma-seperated values : (small, medium, large, xl, 1xl, 2xl, 3xl, s/m, m/l) - also Shoe Sizes 
feed_type | If the user has multiple feeds they post to (for example Divas, Kids, Diva VIP), then you specify them here. Each feed type has a numeric assigned to it (1, 2, 3 respectively)
instock | Boolean true/false - whether or not to return products not available in that size
skip | Default is 0 - use this parameter to acheive pagination. You should deduplicate results based on the prod_id as the primary key

# Cart

## Get Cart

```shell
curl "https://cdn.commentsold.com/divas/api/2.0/user/cart?app_version=1.3"
  -H "Authorization: meowmeowmeow"
```

### HTTP Request

`GET /shop/api/2.0/user/cart`

Parameter | Description
--------- | -------
app_version | The app_version that is built-in with the app launch. This should correspond to a specific release of the app

## Get Past Orders

```shell
curl "https://cdn.commentsold.com/divas/api/2.0/user/orders?skip=0&app_version=1.3"
  -H "Authorization: meowmeowmeow"
```


### HTTP Request

`GET /shop/api/2.0/user/orders`

Parameter | Description
--------- | -------
app_version | The app_version that is built-in with the app launch. This should correspond to a specific release of the app
skip	| Default is 0 - use this parameter to acheive pagination. You should deduplicate results based on the order_id as the primary key

## Add Item

```shell
curl "https://cdn.commentsold.com/divas/api/2.0/user/cart"
  -H "Authorization: meowmeowmeow"
```

### HTTP Request

`POST /shop/api/2.0/user/cart/products`

Parameter | Description
--------- | -------
app_version | The app_version that is built-in with the app launch. This should correspond to a specific release of the app
id |
inventory_id |


## Delete Item


### HTTP Request

`DELETE /shop/api/2.0/user/cart/products/<orders_products_id>`

Parameter | Description
--------- | -------
app_version | The app_version that is built-in with the app launch. This should correspond to a specific release of the app

## Add Coupon


### HTTP Request

`POST /shop/api/2.0/user/cart/coupon/<coupon_id>`

Parameter | Description
--------- | -------
app_version | The app_version that is built-in with the app launch. This should correspond to a specific release of the app
code | Unique Coupon Code

<aside class="notice">This should be a blocking request - refresh the cart when operation returns.</aside>

# Stripe

## Get All Cards


### HTTP Request

`GET /shop/api/2.0/user/cards`

Parameter | Description
--------- | -------
app_version | The app_version that is built-in with the app launch. This should correspond to a specific release of the app

> The above command returns JSON structured like this:

No cards:
```json
{
  []
}
```

Cards:
```json
{
  ["cards"]

}
```

## Add Card


### HTTP Request

`POST /shop/api/2.0/user/cards`

Parameter | Description
--------- | -------
app_version | The app_version that is built-in with the app launch. This should correspond to a specific release of the app
id |

## Delete Card


### HTTP Request

`DELETE /shop/api/2.0/user/cards/<ID>`

Parameter | Description
--------- | -------
app_version | The app_version that is built-in with the app launch. This should correspond to a specific release of the app

## Set Default Card


### HTTP Request

`PUT /shop/api/2.0/stripe/cards/<ID>`

Parameter | Description
--------- | -------
app_version | The app_version that is built-in with the app launch. This should correspond to a specific release of the app

# Checkout

## Stripe



### HTTP Request

`POST /shop/api/2.0/checkout/stripe`

Parameter | Description
--------- | -------
app_version | The app_version that is built-in with the app launch. This should correspond to a specific release of the app
oid |
amount |
token |

<aside class="notice">This should be a blocking request.</aside>

## PayPal


### HTTP Request

`POST /shop/api/2.0/checkout/paypal`

Parameter | Description
--------- | -------
app_version | The app_version that is built-in with the app launch. This should correspond to a specific release of the app
oid |
paymentjson |

<aside class="notice">This should be a blocking request.</aside>

## Get Balance


### HTTP Request

`GET /shop/api/2.0/user/balance`

Parameter | Description
--------- | -------
app_version | The app_version that is built-in with the app launch. This should correspond to a specific release of the app

## Use Balance



### HTTP Request

`POST /shop/api/2.0/user/balance`

Parameter | Description
--------- | -------
app_version | The app_version that is built-in with the app launch. This should correspond to a specific release of the app

> The above API call can do enough to mark an order as complete. We will mark the order as paid if the balance being used is greater than the total of the order

200 OK
```json
{"success": "Credit successfully applied to the order"}
```

# Settings

## Set Address



### HTTP Request

`POST /shop/api/2.0/address`

Parameter | Description
--------- | -------
app_version | The app_version that is built-in with the app launch. This should correspond to a specific release of the app
street |
apt |
state |
city |
zip |

## Get Address


### HTTP Request

`GET /shop/api/2.0/address`

Parameter | Description
--------- | -------
app_version | The app_version that is built-in with the app launch. This should correspond to a specific release of the app

## Update Email


### HTTP Request

`PUT /shop/api/2.0/email`

Parameter | Description
--------- | -------
app_version | The app_version that is built-in with the app launch. This should correspond to a specific release of the app
email |


## Get Email


### HTTP Request

`GET /shop/api/2.0/email`

Parameter | Description
--------- | -------
app_version | The app_version that is built-in with the app launch. This should correspond to a specific release of the app
