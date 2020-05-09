# App library documentation
The libraries for all languages and frameworks are as similar as possible. Differences can be found in [language specific instructions](/languages/), so make sure to read your languages page there first.

## API initialization
Before being able to use the API, it has to be initialized:

<!-- tabs:start -->

#### ** Go **
```go
import (
    "github.com/hopperteam/hopper-api" hopperApi
)

api := hopperApi.CreateHopperApi(HopperProd)
api := hopperApi.CreateHopperApi(HopperDev)
```
#### ** Python **
```python
import hopper-api

api = hopper_api.HopperApi(hopper_api.HopperDev)
apiProd = hopper_api.HopperApi(hopper_api.HopperProd)
```

<!-- tabs:end -->

The parameter to the function / constructor specifies whether to use Hopper's [development instance](https://dev.hoppercloud.net) or Hopper's [production instance](https://app.hoppercloud.net). You should use the development instance as long as your app does not go into production and is user-facing.

This function returns / creates a `HopperApi` instance, which is responsible for the communication to Hopper. Creation of this object does not check whether Hopper is reachable. This can be accomplished with `HopperApi.checkConnectivity()`:

<!-- tabs:start -->

#### ** Go **
```go
if !api.CheckConnectivity() {
    // Cannot reach Hopper :(
}
```
#### ** Python **
```python
if not api.check_connectivity():
    # Cannot reach Hopper! :( 
```

<!-- tabs:end -->

Please note: This function call is not mandatory. It is just good to check whether the API is correctly installed and network routes are correctly configured.

## Create an App
You'll have to create an `App` object once for your app. After that you should serialize and deserialize it. Creation of an App is accomplished by `HopperApi.createApp()`:

<!-- tabs:start -->

#### ** Go **
```go
app, err := api.CreateApp("Hopper", "hoppercloud.net", "https://hoppercloud.net/logo.png", "https://hoppercloud.net/manageSubscription", "info@hoppercloud.net")

if err != nil {
    // An error occured :(
}
```
#### ** Python **
```python
try:
    app = api.create_app("Hopper", "hoppercloud.net", "https://hoppercloud.net/logo.png", "https://hoppercloud.net/manageSubscription", "info@hoppercloud.net")
except:
    # An error occured! :(
    pass
```

## Update an App
You can also update some of the Apps metadata:
#### ** Go **
```go
err := app.Update(&AppUpdate{
    Name: "App2",
    ImageUrl: "..",
    ManageUrl: "..",
    ContactEmail: ".."
})

if err != nil {
    // An error occured :(
}
```
#### ** Python **
```python
try:
    app.update(name = "App2", imageUrl = "..", manageUrl = "..", contactEmail = "..")
except:
    # An error occured! :(
    pass
```
<!-- tabs:end -->
You can also leave out parameters, you should only specify parameters you want to change.
