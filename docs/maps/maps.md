---
layout: default
title: Maps
nav_order: 8
---

# Heremap with react native

## Require

- To use heremap requires app_id and app_code for both android and ios, this key will be generated when creating hereamap account

## References

https://github.com/goparrot/geocoder

https://www.npmjs.com/package/heremap

https://github.com/Josh-ES/react-here-maps

## Installation

```sh
    $ npm i @goparrot/geocoder reflect-metadata axios
```

## Usage

```ts
import "reflect-metadata"
import {
  Location,
  Geocoder,
  HereProvider,
  LoggerInterface,
} from "@goparrot/geocoder"
import Axios, { AxiosInstance, AxiosRequestConfig, AxiosResponse } from "axios"

// You can use any logger that fits the LoggerInterface
const logger: LoggerInterface = console

// Set timeout for all requests
const axios: AxiosInstance = Axios.create({
  timeout: 5000,
})

// You can log all requests
axios.interceptors.request.use((request: AxiosRequestConfig) => {
  logger.debug("api request", request)

  return request
})

// You can log all responses
axios.interceptors.response.use((response: AxiosResponse): AxiosResponse => {
  logger.debug(`api response ${response.status}`, response.data)

  return response
})

/**
 * Caching adapter for axios. Store request results in a configurable store to prevent unneeded network requests.
 * @link {https://github.com/RasCarlito/axios-cache-adapter}
 */

const provider: HereProvider = new HereProvider(
  axios,
  "YOUR_APP_ID",
  "YOUR_APP_CODE"
)

const geocoder: Geocoder = new Geocoder(provider)
geocoder.setLogger(logger)
;(async () => {
  try {
    const locations: Location[] = await geocoder.geocode({
      // accuracy: AccuracyEnum.HOUSE_NUMBER,
      address: "1158 E 89th St, Chicago, IL 60619, USA",
      countryCode: "US",
      // postalCode: '60619',
      // state: 'Illinois',
      // stateCode: 'IL',
      // city: 'Chicago',
      // language: 'en', // default
      // limit: 5, // default
      // fillMissingQueryProperties: true, // default
      withRaw: true, // default false
    })

    logger.info("locations", locations)
  } catch (err) {
    logger.error(err)
  }

  try {
    const locations: Location[] = await geocoder.reverse({
      // accuracy: AccuracyEnum.HOUSE_NUMBER,
      lat: 41.7340186,
      lon: -87.5960762,
      countryCode: "US",
      // language: 'en', // default
      // limit: 5, // default
      // withRaw: false, // default
    })

    console.info("locations", locations)
  } catch (err) {
    console.error(err)
  }
})()
```

## Special Geocoders and Providers

- The ProviderAggregator is used to register several providers so that you can manualy decide which provider to use later on.

```tsx
import "reflect-metadata"
import Axios, { AxiosInstance } from "axios"
import {
  Location,
  GoogleMapsProvider,
  HereProvider,
  ProviderAggregator,
  MapQuestProvider,
} from "@goparrot/geocoder"

const axios: AxiosInstance = Axios.create({
  timeout: 5000,
})

const geocoder: ProviderAggregator = new ProviderAggregator([
  new MapQuestProvider(axios, "YOUR_API_KEY"),
  new HereProvider(axios, "YOUR_APP_ID", "YOUR_APP_CODE"),
])

geocoder.registerProvider(new GoogleMapsProvider(axios, "YOUR_API_KEY"))
;(async () => {
  try {
    const locations: Location[] = await geocoder
      .using(GoogleMapsProvider)
      .geocode({
        address: "1158 E 89th St, Chicago, IL 60619, USA",
      })

    console.info(locations)
  } catch (err) {
    console.error(err)
  }
})()
```

- The ProviderAggregator's API is fluent, meaning you can write:

```ts
const locations: Location[] = geocoder
  .registerProvider(new MyCustomProvider(axios))
  .using(MyCustomProvider)
  .geocode({
    // ...
  })
```

- The using() method allows you to choose the provider to use by its class name. When you deal with multiple providers, you may want to choose one of them. The default behavior is to use the first one, but it can be annoying.

## Example

- Display the map (There are different ways to display the map) in this tutorial using Mapview from react-native-maps.

- The map used here is google map.

- Use `<Polyline /> ` to get route.

```tsx
<>
  <MapView
    provider="google"
    style={[styles.map, { width: SCREEN_WIDTH - 32 }]}
    initialRegion={selectedRegion}
    region={selectedRegion}
    showsMyLocationButton={false}
    showsUserLocation={true}
    showsBuildings={true}
    showsCompass={true}
    showsScale={true}
    onLongPress={handleMapLongPress}
    onMarkerDrag={handleMapLongPress}
  >
    <Marker coordinate={selectedRegion} draggable={true} title={store.address}>
      {Children}
    </Marker>
    <Polyline
      coordinates={routeLocation.routeForMap}
      strokeWidth={2}
      strokeColor="red"
      geodesic={true}
    />
  </MapView>
</>
```
