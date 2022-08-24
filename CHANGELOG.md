# Changelog

## Version [1.0.1] - 2022-08-01

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


## Version [v1.0.0] - 2022-04-18

- Initial release.
