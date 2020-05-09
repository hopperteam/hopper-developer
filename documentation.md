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

api = hopper_api.HopperApi(hopperApi.HopperDev)
apiProd = hopper_api.HopperApi(hopperApi.HopperProd)
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

