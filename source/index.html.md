---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - javascript

toc_footers:
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the SEND API! You can use our API to access our delivery endpoints which gives you the ability create and track deliveries or shipments.

This documentation is written for shell and javascript but will work for other languages that support RESTful API consumption. 


# Authentication

> To authorize, use this code:

```shell

  curl 'api_endpoint_here' 
  -H 'x-api-key: test_or_production_key' 
  -H 'Content-Type: application/json'
  -X GET #POST/PUT/DELETE
```

```javascript
fetch('api_endpoint_here', {
  method: GET,//POST/PUT/DELETE,
  headers: {
    'x-api-key': 'test_or_production_api_key',
    'Content-Type': 'application/json'
  }
}).then(res => res.json()).then((result)=> {
  //Result of API call
}).catch((error)=> {
  //An error occurred
})
```

> Make sure to replace `test_or_production_api_key` with your API key.

The SEND API requires your API key as a header parameter. The API key should be passed under “x-api-key” for authentication purposes. 

SEND expects for the API key to be included in all API requests to the server in a header that looks like the following:

`x-api-key: api_key`

<aside class="notice">
You must replace <code>api_key</code> with your personal API key.
</aside>

Use Test API key for test requests and Production API key for production requests.

# Requests

The base URL for all requests to the Postmates API is: <code>https://api.send.ng/</code>

## Our Restful API:
- uses standard HTTP verbs like GET, POST, DELETE.
- uses standard HTTP error responses to describe errors.
- authentication is specified with HTTP Basic Authentication.
 POST data should be encoded as a standard <code>application/json</code>.

##  Versioning

Versioning allows us to provide developers a consistent experience. We provide two levels of versioning:

- Resource: All endpoints are prefixed with a version such as <code>/v1</code>. This version refers to the overall layout of the endpoints and response standards.

## Address Format

Please format pickup and destination addresses this way:
<code>Street Address, City, State, country, Zip</code>

Any other information, like apartment number, door codes, "care of" instructions, should all be added to pick up or destination notes fields.

## Responses
All responses are in JSON.

1. 200 - OK: Everything went as planned.
2. 304 - Not Modified: Resource hasn't been updated since the date provided. See Caching below.
3. 400 - Bad Request: Something went wrong, check below for more error messages
4. 401 - Unauthorized: Authentication was incorrect.
5. 404 - Not Found
6. 429 - Too Many Requests
7. 500 - Internal Server Error: We had a problem processing the request.
8. 503 - Service Unavailable: Try again later.

# Test Mode
> Expected Response Object


```shell

 #Response
  {
    job:{
    origin_name: "John Doe", 
    origin_full_address: "1 Infinite Loop, CA, USA",
    origin_street_name: "1 Infinite Loop",
    origin_city: "Cupertino",
    origin_state: "CA",
    origin_country: "USA",
    origin_mobile: "+1302343309",
    company_id:1,
    origin_comment: "Check out for more information",
    pickup_time: "2019-06-25 12:00pm",
    pickup_day: "2019-06-25",
    vehicle: "bike"
  },
  drops:[
      {
        dest_name: "Jane Doe", 
        dest_full_address: "5 Infinite Loop, CA, USA",
        dest_street_name: "5 Infinite Loop",
        dest_city: "Cupertino",
        dest_state: "CA",
        dest_country: "USA",
        dest_mobile: "+10232343454",
        dest_comment: "Drop outside",
        pickup_time: "2019-06-25 8:00am",
        weight : 6
        eta: "Estimated delivery date",
        job_price: "Price of delivery in NGN",
        token: "Token used for tracking delivery"
      }],
      status: 200 
  }

```

```javascript
  //Response
  {
    job:{
    origin_name: "John Doe", 
    origin_full_address: "1 Infinite Loop, CA, USA",
    origin_street_name: "1 Infinite Loop",
    origin_city: "Cupertino",
    origin_state: "CA",
    origin_country: "USA",
    origin_mobile: "+1302343309",
    company_id:1,
    origin_comment: "Check out for more information",
    pickup_time: "2019-06-25 12:00pm",
    pickup_day: "2019-06-25",
    vehicle: "bike"
  },
  drops:[
      {
        dest_name: "Jane Doe", 
        dest_full_address: "5 Infinite Loop, CA, USA",
        dest_street_name: "5 Infinite Loop",
        dest_city: "Cupertino",
        dest_state: "CA",
        dest_country: "USA",
        dest_mobile: "+10232343454",
        dest_comment: "Drop outside",
        pickup_time: "2019-06-25 8:00am",
        weight : 6
        eta: "Estimated delivery date",
        job_price: "Price of delivery in NGN",
        token: "Token used for tracking delivery"
      }],
      status: 200 
  }
  
```
For your development convenience, we provide a test mode for our API. Using a test API key will allow you to exercise all the endpoints. We will automate the lifecycle of deliveries to provide a simulated experience.

Here is an example of a post request body


<code> "job": {
"origin_name": "John Doe", 
"origin_full_address": "1 Infinite Loop, CA, USA",
"origin_street_name": "1 Infinite Loop",
"origin_city": "Cupertino",
"origin_state": "CA",
"origin_country": "USA",
"origin_mobile": "+1302343309",
"company_id":1,
"origin_comment": "Check out for more information",
"pickup_time": "2019-06-25 12:00pm",
"pickup_day": "2019-06-25",
"vehicle": "bike",
"drops_attributes": [{
"dest_name": "Jane Doe", 
"dest_full_address": "5 Infinite Loop, CA, USA",
"dest_street_name": "5 Infinite Loop",
"dest_city": "Cupertino",
"dest_state": "CA",
"dest_country": "USA",
"dest_mobile": "+10232343454",
"dest_comment": "Drop outside",
"pickup_time": "2019-06-25 8:00am",
"weight" : 6
}]
}
</code>



# Endpoints

# Quotes

## Get A Quote

POST <code>/api/v1/quotes</code>

> Create a quote

```shell
curl '/api/v1/quotes'
-H 'Content-Type: application/json'
-H 'x-api-key: test_or_production_api_key'
-d '@data.json' #job object in a json file
-X POST

#Successful request
{
  price: 'Estimated price of delivery' #float
  status: 200
}
```

```javascript
  //Set job object
  let job = {
    //Origin and destination details as REQUIRED
  }
  fetch('/api/v1/quotes', {
    method: 'POST',
    body: JSON.stringify(job),
    headers: {
      "Content-Type": "application/json",
      "x-api-key": "test_or_production_api_key"
    }
  }).then(res => res.json()).then(result => {
    if (result.status === 200) {
      //Request successfull.
        result = {
          price: 'Estimated price of delivery' //float,
          status: 200
        }
    }else{
      //An error occurred due to invalid parameters
    }
  }).catch(e => {
    //An unexpected error occurred (server down?)
    console.log(e)
  });
```

The quotes API returns the estimated price for a shipment given the weight of the shipment, its origin, and destination. Drop types are determined by origin and destination parameters.

**Query Parameters**

Name | Data Type | Description
--------- | ------- | -----------
drop_type | string | “intl”, “nationwide”, or “lastmile”
weight REQUIRED | float | The weight of the shipment
dest_country REQUIRED | string | Drop off country, if drop_type is “intl”
origin_country REQUIRED | string | Pickup country, if drop_type is “intl”
dest_state REQUIRED | string | Drop off state, if drop_type is “nationwide”
dest_city REQUIRED | string | Drop off city, if drop_type is “nationwide” or“lastmile”
origin_state REQUIRED | string | Pickup state, if drop_type is “nationwide”
origin_city REQUIRED | string | Pickup city, if drop_type is “nationwide”

# Deliveries

## Create a delivery
> Create a delivery

```shell
curl '/api/v1/deliveries'
-H 'Content-Type: application/json'
-H 'x-api-key: test_or_production_api_key'
-d '@data.json' #job object in a json file
-X POST

#See response as shown in test mode section

```

```javascript
  //Set job object
  let job = {
    //Job details as shown in test mode section
  }
  fetch('/api/v1/deliveries', {
    method: 'POST',
    body: JSON.stringify(job),
    headers: {
      "Content-Type": "application/json",
      "x-api-key": "test_or_production_api_key"
    }
  }).then(res => res.json())
  .then(result => {
    if (result.status === 200) {
      //Request successfull.
      //See response as shown in test mode section
    }else{
      //An error occurred due to invalid parameters
    }
  }).catch(e => {
    //An unexpected error occurred (server down?)
    console.log(e)
  });
```

POST:  <code>/api/v1/deliveries/</code>

**Query Paramters**

Name | Data Type | Description
--------- | ------- | -----------
dest_full_address REQUIRED | string | The dropoff address for a potential delivery.
origin_address_address REQUIRED | string | The pickup address for a potential delivery.
dest_lat | float | Latitude of dropoff location will be used instead of geocoding address if passed 
dest_long | float | Longitude of dropoff location, will be used instead of geocoding address if passed.
dest_mobile | string | Phone number of the dropoff location. If passed, the phone number will be validated.
origin_lat | float | Latitude of origin location, will be used instead of geocoding address if passed
origin_longitude | float  | Longitude of the pickup location will be used instead of geocoding address if passed.
pickup_day REQUIRED | timestamp (RFC 3339) | Date/Time (YYYY-MM-DD HH:MM: SS ) for when a delivery is ready to be picked up.
origin_mobile | string | Phone number of the pickup location. If passed, the phone number will be validated.
pickup_time REQUIRED | timestamp (RFC 3339) | Date/Time for when an order will be ready for pickup
Weight REQUIRED | float | Weight of items to be picked up and delivered.
dest_country REQUIRED | string | Destination country.
dest_city REQUIRED | string | City items will be delivered to.
dest_state REQUIRED | string | State items will be delivered to.
origin_country REQUIRED | string | Origin country.
origin_city REQUIRED | string | City items will be picked up from.
origin_state REQUIRED | string State items will be picked up from.
origin_name REQUIRED | string | Name of sender 
dest_name REQUIRED | string | Name of receiver
origin_comment | string | Extra comments for pickup information
dest_comment | string | Comment for delivery. Delivery instructions
vehicle | string | Type of vehicle necessary for pickup and delivery.

## Get All Deliveries

```shell
curl '/api/v1/deliveries'
-H 'Content-Type: application/json'
-H 'x-api-key: test_or_production_api_key'
-X GET

#Result is a list of drops depending on specified filter

```

```javascript
fetch('/api/v1/deliveries', {
    headers: {
      "x-api-key": "test_or_production_api_key"
      "Content_Type": "application/json"
    }
  }).then(res => res.json())
  .then(result => {
      console.log(result)// List of drops depending on specified filter
  }).catch(e => {
    //An unexpected error occurred (server down?)
    console.log(e)
  });
```
List all deliveries for a customer.

**Query Parameters**

<code>completed</code>  => true if to filter by completed
<code>current</code>  => true if to filter by current

<code>page</code> => page of items to show (used for pagination)


## Get A Delivery

```shell
curl '/api/v1/deliveries/{drop-token}'
-H 'Content-Type: application/json'
-H 'x-api-key: test_or_production_api_key'
-X GET

#Result is a drop object containing updated details of a drop

```

```javascript
fetch('/api/v1/deliveries/{drop-token}', {
    headers: {
      "x-api-key": "test_or_production_api_key"
    }
  }).then(res => res.json())
  .then(result => {
     console.log(result) //Drop object containing updated details of a drop
  }).catch(e => {
    //An unexpected error occurred (server down?)
    console.log(e)
  });
```
GET:  <code>/api/v1/deliveries/{drop_token}</code>


Get updated details about a shipment

**Query Parameters**
<code>drop-token</code> => Unique identifier for a shipment

## Cancel A Delivery

```shell
curl '/api/v1/deliveries/{drop-token}/cancel'
-H 'Content-Type: application/json'
-H 'x-api-key: test_or_production_api_key'
-X DELETE

#Result
{
  message: "Status of delivery" #string,
  status: 200 #Status code
}

```

```javascript
fetch(' /api/v1/deliveries/{drop_token}/cancel', {
    method: 'DELETE',
    headers: {
      "x-api-key": "test_or_production_api_key",
      "Content-Type": "application/json"
    }
  }).then(res => res.json())
  .then(result => {
    if (result.status === 200){
      console.log(result.message) //Status of delivery
    }
  }).catch(e => {
    //An unexpected error occurred (server down?)
    console.log(e)
  });
```
DELETE:  <code> /api/v1/deliveries/{drop_token}/cancel</code>


Cancel an ongoing delivery. Delivery can only be canceled prior to a courier completing pickup. Delivery fees still apply.

**Query Parameters**

<code>drop-token</code> => Unique identifier for a shipment

## Track A Delivery

```shell
curl '/api/v1/deliveries/{drop-token}/track'
-H 'Content-Type: application/json'
-H 'x-api-key: test_or_production_api_key'
-X GET

#Result
{
  #Tracking details of a delivery
}

```

```javascript
fetch('/api/v1/deliveries/{drop_token}/track', {
    headers: {
      'x-api-key": "test_or_production_api_key',
      'Content-Type: application/json'
    }
  }).then(res => res.json())
  .then(result => {
    if (result.status === 200){
      console.log(result) //Tracking details
    }
  }).catch(e => {
    //An unexpected error occurred (server down?)
    console.log(e)
  });
```
GET:  <code> /api/v1/deliveries/{drop_token}/track</code>


Get tracking details of a delivery.

**Query Parameters**

<code>drop-token</code> => Unique identifier for a shipment