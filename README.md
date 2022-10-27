# FedEx Shipment System API
In the current revision, this repository contains a system API for abstracting the FedEx Ship API. This FedEx Shipment System API is a reusable asset that enables developers to create FedEx return shipments without any need to learn the FedEx API. In other words, it provides a means of insulating developers from the complexity of integrating with FedEx.

> :exclamation: **FedEx requires a developer account to use their API.** You can sign up for free on the FedEx Developer Portal here: https://developer.fedex.com/api/en-us/register.html

## Table of Contents
- [Technology Stack Overview](#technology-stack-overview)
- [Implementation Overview](#implementation-overview)
- [Instructions](#instructions)
- [Sample Request Message](#sample-request-message)
- [Reporting Issues](#reporting-issues)
- [Additional Resources](#additional-resources)

## Technology Stack Overview
We implemented this system API using the following technology stack:
- MuleSoft Anypoint Studio 7.13
- Mule Server 4.4.0
- FedEx API Authorization v1 (Last Updated 07/27/2020)
- FedEx Ship API v1 (Last Updated 07/27/2020)

Although not formally tested, you could easily use older or newer versions of the MuleSoft Anypoint Studio, Mule Server, or both.

## Implementation Overview
As of this writing, the FedEx Shipment System API implements a single operation for creating a return shipment label. We stripped down the request body of the POST method to include only the strict minimum required properties. Furthermore, in the current revision, we send the request body you submit to the FedEx Shipment System API as-is to the FedEx Ship API.

As it is common practice at MuleSoft, we leverage RAML to specify our API. Primarily for convenience and to make sharing the Anypoint Studio project easier, we included the RAML file in the project (`src/main/resources/api`) rather than having a dependency to its published specification in Anypoint Exchange.

## Instructions
1. Download this repo or clone it to your Anypoint Studio workspace.
    ```sh
    git clone https://github.com/abelisle-mulesoft/fedex-shipment-sapi.git
    ```

2. Edit `src/main/resources/properties/configuration.yaml` and at a bare minimum, configure the following properties:
   - `fedex.authorization.client_id`,  which is your FedEx client id.
   - `fedex.authorization.client_secret`,  which is your FedEx client secret.

3. Compile and run the project in Anypoint Studio as a smoke test. Optionally, test the FedEx Shipment System API using the API Console in Anypoint Studio or your preferred API testing tool (e.g., Postman).

## Sample Request Message
We successfully used the following sample request message for testing the FedEx Shipment System API.
> :warning: **Before using this sample message,** you must update the account number in two accountNumber properties.
```json
{
    "mergeLabelDocOption": "LABELS_AND_DOCS",
    "requestedShipment": {
        "shipper": {
            "address": {
                "streetLines": [
                    "10 FedEx Parkway",
                    "Suite 302"
                ],
                "city": "Beverly Hills",
                "stateOrProvinceCode": "CA",
                "postalCode": "90210",
                "countryCode": "US"
            },
            "contact": {
                "personName": "John Taylor",
                "phoneNumber": "2065551212"
            }
        },
        "recipients": [
            {
                "address": {
                    "streetLines": [
                        "10 FedEx Parkway",
                        "Suite 302"
                    ],
                    "city": "Beverly Hills",
                    "stateOrProvinceCode": "CA",
                    "postalCode": "90210",
                    "countryCode": "US"
                },
                "contact": {
                    "personName": "John Taylor",
                    "phoneNumber": "3125551212"
                }
            }
        ],
        "pickupType": "DROPOFF_AT_FEDEX_LOCATION",
        "serviceType": "PRIORITY_OVERNIGHT",
        "packagingType": "FEDEX_BOX",
        "shippingChargesPayment": {
            "paymentType": "SENDER",
            "payor": {
                "responsibleParty": {
                    "accountNumber": {"value": "#########"}
                }
            }
        },
        "shipmentSpecialServices": {
            "specialServiceTypes": ["RETURN_SHIPMENT"],
            "returnShipmentDetail": {
              "returnEmailDetail": {
                "merchantPhoneNumber": "9015551212"
              },
              "returnType": "PRINT_RETURN_LABEL"
            }
        },
        "labelSpecification": {
            "labelStockType": "PAPER_85X11_TOP_HALF_LABEL",
            "imageType": "PDF"
        },
        "requestedPackageLineItems": [
            {
                "weight": {
                    "units": "LB",
                    "value": 1
                }
            }
        ]
    },
    "labelResponseOptions": "URL_ONLY",
    "accountNumber": {"value": "#########"}
}
```

## Reporting Issues
You can report new issues at this link https://github.com/abelisle-mulesoft/fedex-shipment-sapi/issues

## Additional Resources
Following are references and additional resources:
- FedEx Developer Portal: https://developer.fedex.com/api/en-us/home.html
- FedEx API Getting Started: https://developer.fedex.com/api/en-us/get-started.html
- FedEx API Catalog: https://developer.fedex.com/api/en-us/catalog.html
- FedEx API Authorization: https://developer.fedex.com/api/en-us/catalog/authorization.html
- FedEx Ship API: https://developer.fedex.com/api/en-us/catalog/ship.html
