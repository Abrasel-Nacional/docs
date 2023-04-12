# Changelog

## Version [1.1.1] - Apr 3, 2023

#### ORDERS changes:

- [GET /v1/orders/{orderId}](#operation/ordersDetails)

    - added `sendDelivered` property. This property should have been added together with the [POST /v1/orders/{orderId}/delivered](#operation/orderDelivered) endpoint  

    - added `CANCELLATION_DENIED` to the order events. This event was described in the documentation in the [Orders Cancellation](#tag/ordersCancellation) section but was not present in the enum.

- [POST /v1/orders/{orderId}/requestCancellation](#operation/requestCancellation)

    - fixed the names of the following reasons:
      - `RESTAURANT_WITHOUT_DELIVERY_MAN` to `RESTAURANT_WITHOUT_DELIVERY_PERSON`
      - `INTERNAL_DIFFICULTIES_OF THE RESTAURANT` to `INTERNAL_DIFFICULTIES_OF_THE_RESTAURANT`

## Version [1.1.0] - Jan 23, 2023

#### MERCHANT changes:

- [GET /v1/merchant](#operation/getMerchant)

  - added new optional propertie `acceptedCards` in the `basicInfo` entity of endpoint.

      This field is intended to indicate which card brands are accepted by the merchant.

  - added an enumerator to propertie `unit` in the `items` entity of endpoint [GET /v1/merchant](#operation/getMerchant).

  - added new optional field `targetAppId` in the `service` entity of endpoint [GET /v1/merchant](#operation/getMerchant).

      This field is intended to indicate a specific Ordering Application to receive the service information.

- [POST /v1/merchantUpdated](#operation/menuUpdated)

  - added `MERCHANT` and `BASIC_INFO` options to the enumerator of the `entityType` field of the webhook.

    This allows the webhook to now receive the full merchant entity information and can be used instead of the [GET /v1/merchant](#operation/getMerchant) endpoint.  
    This can be used by **Software Services** that do not have the infrastructure to expose this endpoint.

    It is also possible to update any entity that is part of the Merchant object. (this was already possible previously with the exception of BASIC_INFO entity).

#### ORDERS changes:

- added a new optional order event, called `DELIVERED`.  

  With this, the following changes have been made:

  - [GET /v1/orders/{orderId}](#operation/ordersDetails)
    - added `DELIVERED` to the events

  - added new [POST /v1/orders/{orderId}/delivered](#operation/orderDelivered) endpoint

    This endpoint is intended to indicate to the **Ordering Application** that an order has been delivered.

- [GET /v1/orders/{orderId}](#operation/ordersDetails)

  - added more options to the enumerator of propertie `unit` in the `items` entity of the endpoint, reflecting the created enum.

  - added new field `lastEvent` to be filled with the last valid event sent/polled (whether acknowledged or not).

  - added new optional field `brand` to the `payments>methods` entity.

    This field is intended to indicate the brand of the card (for cases where the chosen payment method has a brand).

- [POST /v1/orders/{orderId}/requestCancellation](#operation/requestCancellation)

  - added propertie `cancellationStatus` to the 422 response body to indicate the result of the cancellation request in case of a duplicate event.

- [POST /v1/orders/{orderId}/confirm](#operation/confirmOrder)

  - added new optional field `preparationTime` to endpoint .

    This field is used to indicate the estimated time to prepare the order.

#### LOGISTICS changes:

- the logistics standard is no longer BETA, and is now versioned together with the RELEASE version.

      
      
## Version [1.0.1] - Aug 1, 2022

  - Added the possibility to indicate orders with scheduled delivery.  
  With this the following changes were made:

    - [GET /merchant](#operation/getMerchant)
      - `service` > Added a new optional field `serviceTiming`, where the merchant can indicate the delivery timing available for that service.

      ```
      ...
      "serviceTiming":
          {
              "timing": ["INSTANT", "SCHEDULED"],
              "schedule":
                  {
                      "scheduleTimeWindow": "15_MINUTES",
                      "scheduleStartWindow": "15_MINUTES",
                      "scheduleEndWindow": "15_MINUTES",
                  },
      ...
      ```
    - [GET /orders/{orderId}](#operation/ordersDetails)
      - Added `SCHEDULED` option to the `orderTiming` field.

      - Added a new property called `schedule` where the delivery window for scheduled orders will be specified with the following structure:
        ```
            {
              ...
              "schedule": {
                "scheduledDateTimeStart": "2019-08-24T14:15:22Z",
                "scheduledDateTimeEnd": "2019-08-24T14:15:22Z"
                },
              ...
            }
        ```

  - Added a new service type called **INDOOR**.  
    The following places are affected:

    - [GET /merchant](#operation/getMerchant)
      - added option `INDOOR` to `type` field of entity `service`.

    - [GET /orders/{orderId}](#operation/ordersDetails)
      - added new property: `indoor` with the following structure:     

      ```
      {
        ...
        "indoor": {
          "mode": "PLACE",
          "indoorDateTime": "2019-08-24T14:15:22Z",
          "place": "string"
        },
        ...
      }
      ```
  - Added a new endpoint so that **Merchant** can send information related to its [GET /merchant](#operation/getMerchant) endpoint to the **Ordering APPLICATION**:

    - [PUT /v1/merchantOnboarding](#operation/putMerchantOnboarding) 

  - Added a new [Working Versions](#tag/versionsSection) section with endpoints so that both the Software Service and the Ordering Application can check which version of the Open Delivery API the other end is using. The endpoints created were:

    - [GET /v1/versions/orderingApp](#operation/getOrderingAppVersions)
    - [GET /v1/versions/merchant](#operation/getMerchantVersions)

  - Added the `status` property to entities `Item`, `ItemOffer` and `Option` on the [GET /merchant](#operation/getMerchant) endpoint.

  - Added HTTP Status code 422 on the following endpoints:
    - [POST /v1/orders/{orderId}/confirm](#operation/confirmOrder)
    - [POST /v1/orders/{orderId}/readyForPickup](#operation/orderReady)
    - [POST /v1/orders/{orderId}/dispatch](#operation/dispatchOrder)
    - [POST /v1/orders/{orderId}/requestCancellation](#operation/requestCancellation)
    - [POST /v1/orders/{orderId}/acceptCancellation](#operation/cancellationAccepted)
    - [POST /v1/orders/{orderId}/denyCancellation](#operation/cancellationDenied)

  - Added HTTP Status code 204 on the following endpoints:
    - [POST /v1/merchantUpdated](#operation/menuUpdated)
    - [POST /v1/orderUpdate](#operation/newEvent)
    - [POST /v1/orders/{orderId}/acceptCancellation](#operation/cancellationAccepted)
    - [POST /v1/orders/{orderId}/denyCancellation](#operation/cancellationDenied)

  - Removed the **requirement** of the fields:
    - [GET /merchant](#operation/getMerchant)
      - `menu` > `description`
      - `menu` > `disclaimer`
      - `category` > `description`
      - `optionGroup` > `description`

  #### Other Changes:

  - Added a new [Developer Tools](#section/Developer-Tools) section where links to partner tools to help with API development will be listed.

  - [Order Overview](#tag/ordersOverview)

    - The lifecycle and order flow diagrams have been updated to better reflect the service types.

  - Overall revisions of grammatical errors, syntax, examples and descriptions.


## Version [v1.0.0] - Apr 18, 2022

- Initial release.
