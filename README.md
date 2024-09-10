# API Documentation

## POST api/v1/data

## Overview
This API endpoint allows you to send data for processing with a limit of 5 requests per day from the same IP address.

### Request Headers:
- **Accept**: `application/json`

### Authorization:
- None required.

---

## POST /api/v1/data
This endpoint is used to send data for processing.

### Request Body Parameters

- **pluginVersion** (string, required): The version of the plugin.
- **eCommPlatform** (string, required, max: 50 characters): The e-commerce platform being used.
- **omnivaApiKey** (string, required, max: 30 characters): API key for Omniva.
- **senderName** (string, required, max: 50 characters): The name of the sender.
- **senderCountryCode** (string, required, size: 2 characters): The country code of the sender (ISO 3166-1 alpha-2).
- **ordersCount** (object, required): Details about the number of courier and terminal orders.
  - **courier** (integer, required): Number of courier orders.
  - **terminal** (integer, required): Number of terminal orders.
- **ordersCountSince** (string, required, date format: `Y-m-d H:i:s`): Timestamp representing when the orders count started.
- **setPricing** (array, required): Array of pricing details for different countries.
  - **setPricing.*.country** (string, required, size: 2 characters): The country code (ISO 3166-1 alpha-2).
  - **setPricing.*.courier.min** (numeric, nullable): Minimum courier price. Set to `0` for free, or `null`/`""` if not used.
  - **setPricing.*.courier.max** (numeric, nullable): Maximum courier price. Set to `0` for free, or `null`/`""` if not used.
  - **setPricing.*.terminal.min** (numeric, nullable): Minimum terminal price. Set to `0` for free, or `null`/`""` if not used.
  - **setPricing.*.terminal.max** (numeric, nullable): Maximum terminal price. Set to `0` for free, or `null`/`""` if not used.
- **sendingTimestamp** (string, required, date format: `Y-m-d H:i:s`): Timestamp when the data was sent.

---

## Example Request

```json
{
    "pluginVersion": "1.2.2",
    "eCommPlatform": "Opencart v2.0.0",
    "omnivaApiKey": "0123456",
    "senderName": "Test UAB",
    "senderCountryCode": "LT",
    "ordersCount": {
        "courier": 10,
        "terminal": 666
    },
    "ordersCountSince": "2024-08-13 00:00:00",
    "setPricing": [
        {
            "country": "LT",
            "courier": {
                "min": "5",
                "max": "5"
            },
            "terminal": {
                "min": "1",
                "max": "2.5"
            }
        },
        {
            "country": "LV",
            "courier": {
                "min": null,
                "max": null
            },
            "terminal": null
        },
        {
            "country": "EE",
            "courier": {
                "min": "",
                "max": ""
            },
            "terminal": {
                "min": "3.5",
                "max": "3.5"
            }
        },
        {
            "country": "FI",
            "courier": {
                "min": "10",
                "max": "10"
            },
            "terminal": {
                "min": "15.25",
                "max": "15.25"
            }
        }
    ],
    "sendingTimestamp": "2024-08-13 11:04:25"
}
```

---

## Example Response

```json
{
    "status": "OK",
    "code": 200,
    "message": "Data for customer - 0123456 (Test UAB) stored successfully",
    "date": "2024-09-06 14:31:54"
}
```

### Response Properties
- **status** (string): Status of the request.
- **code** (integer): HTTP status code.
- **message** (string): A success or failure message.
- **date** (string): Timestamp of the response (UTC format).
