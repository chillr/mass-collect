# Mass collect API
Mass collect api can be used by a vendor to send bill collect request to his  users. The users need to have Chillr app installed to make the payment.

There are two api's for mass collect.

1. To create a batch containing payment information and receiver information.
2. To check the status of a batch, ie. How many successfully paid and how many are pending or failed due to some reason.

## Create batch

URL: **https://onlineapi.chillr.in/api/v5/collect**

METHOD: **POST**

Parameters:

api_key - Your API key (mandatory)

api_secret_key - Your API secret key (mandatory)

name - Name of the batch (mandatory)

expiry_date - Expiry date for the batch (dd-mm-yyyy) (mandatory)

customers  - Array of customer details (mandatory)

Sample request structure

```json
{
    "api_key" : "5695d71a6368690df2030000",
    "api_secret_key" : "f333884a36ccfc54787e6d96eac5e3d3",
    "name" : "Electricity bill",
    "expiry_date" :  "27-05-2016",
    "customers" : [
      { "msisdn" : "9447741462", "amount" : "200" }, 
      { "msisdn" : "9633789451", "amount" : "600" }
    ]
}
```

Sample success response

```json
{
  "status": "success",
  "status_code" : 0,
  "message": "Collect batch creation accepted",
  "data": {
      "batch_id": "57453c1b636869174b000000",
      "non_chillr_customers" : ["9633789451"]
  }
}
```

**batch_id** is the id of the created mass collect batch

Sample failure response

```json
{
    "status": "failure",
    "status_code": 58,
    "message": "Failed to create batch",
    "data": {
        "reason": [
            "customer 2 : mobile number is not present"
        ]
    }
}
```

```json
{
    "status": "failure",
    "status_code": 58,
    "message": "Failed to create batch",
    "data": {
        "reason": ["None of the customers are Chillr users."]
    }
}
```


## Status check API
This  api is used to check the status of a collect batch.

URL: **https://onlineapi.chillr.in/api/v5/check_collect_status**

METHOD: **POST**

Parameters:

| Field | Description | Mandatory |
| -- | -- | -- |
| api_key | Your API key | Yes |
| api_secret_key | Your API secret key  | Yes |
| batch_id | Id of batch | Yes |

Sample request structure

```json
{
    "api_key" : "5695d71a6368690df2030000",
    "api_secret_key" : "f333884a36ccfc54787e6d96eac5e3d3",
    "batch_id" : "5743fcb263686923fc0d0000"
}
```

Sample success response

```json
{
    "status": "success",
    "status_code" : 0, 
    "data": {
        "batch_id": "5743fcb263686923fc0d0000",
        "batch_status": "expired",
        "customers": [
            {
                "msisdn": "9447741462",
                "payment_status": "expired",
                "transaction_id": "614410700416"
            },
            {
                "msisdn": "9633789451",
                "message": "Transaction not generated"
            },
            "non_chillr_customers" : ["9633789451"]
        ]
    }
}
```

Sample failure response

```json
{
    "status": "failure",
    "status_code": 62,
    "message": "Batch not found error"
}
```

### Failure codes
 
50 - api_key param is missing 

51 - api_secret_key param is missing 

52 - name param is missing 

53 - expiry_date param is missing 

54 - customer param is missing 

55 - expiry_date format errro (dd-mm-yyyy) 

56 - api_secret_key mismatch error 

57 - Merchant not found error 

58 - error due invalid parameters 

59 - Internal server error 

60 - batch_id param is missing 

61 - Batch not found 

