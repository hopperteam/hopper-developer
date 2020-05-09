# Hopper API documentation
Hopper's API is a RESTful API exposed in several instances for productive usage (link follows) and development usage [https://api-dev.hoppercloud.net/v1/](https://api-dev.hoppercloud.net/v1).

## Structure
Hopper's API consists of some basic data types, linked to each other: `Notification`, `App`, and `Subscription`.

`App` is Hopper's extension point, representing an external application. It can easily be registered via an API endpoint.

`Subscription` the relationship between apps and users. A user can have multiple subscriptions to a specific app.

`Notifications` objects are the elements displayed in Hopper's user interface to the users. A notification has to be sent to a `Subscription`.

To create a `Subscription`, an app has to forward the user to Hopper, containing a `SubscribeRequest`.

## Authentication
Authentication of apps to the API is accomplished by signing parts of the requests with a RSA private key (only known to the app). During propagation of the app to Hopper, the corresponding public key is sent to the API. 
This public key can be changed later on as long as the current private key is known to the app. When this key is lost, the `App` object is no longer functional.

## Subscription Process
The user has to be forwarded to hopper with a `SubscribeRequest` object. The exact process is explained in [Subscription Process](/subscriptionProcess.md).

## Objects
#### `Notification`
  - `id: string`
  - `heading: string` 
  - `subscription: string`
  - `timestamp: number` _in ms_
  - `imageUrl: string?`
  - `isDone: boolean` 
  - `isSilent: boolean` 
  - `type: string` _currently only `"default"`_ 
  - `content: any` _depends on type_ 
  - `actions: Action[]`

#### `Action`
  - `type: string` _currently only `"submit", "text", "redirect"`_
  - `url: string` _where to POST the result_
  - `markAsDone: boolean` _mark notification as done afterwards?_
  - `text: string` _caption of the action_
 
#### `Action Type`
  - `submit`: OnClick: POST Request to `url`
  - `text`: OnSubmit: POST Request to `url` body `{"text": ...}`
  - `redirect`: OnClick: Redirect user in new tab to `url`

#### `App`
  - `id: string`
  - `name: string`
  - `imageUrl: string`
  - `isHidden: boolean`
  - `baseUrl: string`
  - `manageUrl: string?` _links to notification management of the app_
  - `contactEmail: string` _contact for hopper team in case of problems_
  
#### `Subscription`
  - `id: string`
  - `accountName?: string`
  - `app: App`

#### `SubscribeRequest`
  - `id: number` _App Id_
  - `callback: string(url)`: _Callback to be called after success or failure, has to be in the app's base URL_
  - `accountName?: string`: _The display name for this specific subscription (probably the account name)_
  - `requestedInfos: string[]`

## Endpoints
### Calling the API
All URLs are relative to the API's root. Parameters are passed as query parameters for `GET` and `DELETE` queries and as JSON objects in the body for `POST` and `PUT` calls. When a POST or PUT call only has one parameter, the name of it is omitted and the value of parameter is passed directly.

#### `POST /app (name: string, baseUrl: string, imageUrl: string, manageUrl: string, cert: string)` 
_Registers the app with hopper_

`cert` is a base64 encoded PEM-RSA Public Key. The private key is for authentication of the app to the backend.

#### `PUT /app (id: string, content: string)`  
_Updates the app's information_

`content` is a stringified JSON-Object containing the `verify` and `data` attribute which is base64 encoded.   
`data` is a JSON-Object containing the fields that want to be updated. Updatable fields are:
  - `name`
  - `imageUrl`
  - `manageUrl`
  - `contactEmail`
  - `cert` 
  
`verify` is an encrypted (using the private key of the app) sha256 hash of the stringified `data` object.

#### `POST /notification (subscriptionId: string, notification: Notification)`
_Adds a new notification_

#### `PUT /notification (id: string, notification?*: Notification)`
_Updates a notification_

#### `DELETE /notification (id: string)`
_Deletes a notification_
