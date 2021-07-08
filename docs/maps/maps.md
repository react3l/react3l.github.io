---
layout: default
title: Maps
nav_order: 8
---

# Heremap with react native

## Yêu cầu

- Để có thể sử dụng heremap thì yêu câuf cần có app_id và app_code cho cả android và ios, đây là key được tạo ra khi đăng ký tài khoản here map.

## Tài liệu tham khảo

https://github.com/goparrot/geocoder

https://www.npmjs.com/package/heremap

https://github.com/Josh-ES/react-here-maps

## Cài đặt cho cả android và ios

```sh
    $ npm i @goparrot/geocoder reflect-metadata axios
```

## Sử dụng

- Dưới đây là cách sử dụng các service cơ bản mà heremap cung cấp. Bạn có thể sử dụng bất cứ service nào mà heremap cung cấp như lấy vị trí, tính khoảng cách, lấy địa chỉ, ...

- Để sử dụng HereMap bạn phải chỉ định nhà cung cấp dịch vụ là HereProvider với app_id và app_code tương ứng.

```ts
const provider: HereProvider = new HereProvider(
  axios,
  "YOUR_APP_ID",
  "YOUR_APP_CODE"
)
```

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

- Bạn có thể tuỳ chọn nhà cung cấp dịch vụ ngoài Here.

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

```ts
const locations: Location[] = geocoder
  .registerProvider(new MyCustomProvider(axios))
  .using(MyCustomProvider)
  .geocode({
    // ...
  })
```

- Phương thức using() cho phép bạn có thể chọn nhà cung cấp theo class name của nó. Mặc định là GoogleMapsProvider

## Giao diện

- Có nhiều cách khác nhau để hiển thị bản đồ cho người dùng. Trong hướng dẫn này sử dụng Mapview từ breact-native-maps..

- Map được sử dụng là google map.

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
