# Subscription Process
This document outlines the subscription process, SPs have to go through, to be able to send notifications to users in hopper.

## Prerequisites
The app is required to be reachable in the public internet via HTTPS to be able to receive subscription tokens.

Each app is also required to hold a private RSA key to be able to sign JsonWebTokens. The corresponding public key has to be sent to Hopper for verification.

## Registration
First, the app performs the `POST /app` request to sign up (available in the API-documentation). the certificate is the Base64-PEM-encoded public key of the app. 

## Updates
To update the app, the `PUT /app` request is performed. The `content` object is a JsonWebToken signed with the App's private key. Furthermore, the request body contains a `id` field with the id of the app.

## Subscription Process
To subscribe a user to a app, the app has to create a `SubscribeRequest` for the user. This `SubscribeRequest` is the payload of a JsonWebToken signed with the App's private key. The JWT is the content part of the request. 

The user is then navigated to this URL:
```URL 
  {{hopper-instance}}/subscribe?id={{spId}}&content={{JsonWebToken}}
```
Hopper will verify the SPs identity by decoding the request with the specified public key.

When the identity is verified, the user will see an UI which to login and give the app permission. After that, the subscription is created and a `subscriptionId` is generated. 

The user is navigated to the callback, specified in the `SubscribeRequest`. The user will be navigated via an `GET` request, which will receive 2 of 3 query parameters: 
  - `status`: Either `"success"` or `"error"`
  - `error`: In case of `"error"`: The error message.
  - `id`: In case of `"success"`: The `subscriptionId`
  
After that, the subscription process is done and the app can send notifications to the user using the `subscriptionId`.  
