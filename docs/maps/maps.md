---
layout: default
title: Maps
nav_order: 8
---

# Heremap with react native

## Tham khảo

link: <https://www.npmjs.com/package/heremap>

## Cài đặt

- Cài đặt cơ bản theo link <https://www.npmjs.com/package/heremap>

## Sử dụng

# Mô tả

- Cách sử dụng cơ bản đã được mô tả chi tiết theo link <https://www.npmjs.com/package/heremap>

- Để sử dụng được heremap cần phải có app_id và app_code cho cả android và ios, key này sẽ được tạo khi tạo tài khoản hereamap

- Dưới đây là hướng dẫn để có thể sử dụng heremap:

1. Tạo repository:

- Trong repository có nhiệm vụ gọi tới api tương ứng chức năng cần sử dụng của heremap.

- Trước tiên phải config app_id và app_code nhằm xác thực với here map:

```ts
hm.config({
    app_id: YOUR APP_ID,
    app_code: YOUR APP_CODE,
})
```

- Sau đó sẽ gọi tới api cần sử dụng theo chức năng mong muốn:

Vd: Chức năng truyền vào vị trí latlng và lấy ra được địa chỉ ta sử dụng hàm 'reverseGeocode'

```ts
hm.reverseGeocode([coordinate.latitude, coordinate.longitude] as any)
.then((response) => {
return response;
});
```

- response trả về có dạng:

```json
{
    "location": {},
    "address": {},
    "body": {}
}
```

2. Viết service để lấy dữ liệu cần thiết để sử dụng.

3. Hiển thị map (Có nhiều các để hiển thị map khác nhau) trong bài này sử dụng Mapview từ react-native-maps.

- Dưới đây là 1 ví dụ về việc hiển thị map và chỉ đường trên map bạn có thể đọc thêm từ tài liệu của react-native-maps.

- Map được sử dụng ở đây là google map.

- Việc định tuyến đường đi sử dụng api route của heremap và hiển thị thông qua <Polyline />

```tsx
    <>
        <MapView
            provider="google"
            style={[styles.map, {width: SCREEN_WIDTH - 32}]}
            initialRegion={selectedRegion}
            region={selectedRegion}
            showsMyLocationButton={false}
            showsUserLocation={true}
            showsBuildings={true}
            showsCompass={true}
            showsScale={true}
            onLongPress={handleMapLongPress}
            onMarkerDrag={handleMapLongPress} >
            <Marker
                coordinate={selectedRegion}
                draggable={true}
                title={store.address}
            >
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

# Ví dụ

- Trong bài hướng dẫn này, hướng dẫn 2 chức năng cơ bản, các chức năng khác tương tự:

* Lấy địa chỉ khi truyền vào latlng:

public async getLocationAddressByCoordinate(
coordinate: GeoCoordinates,
): Promise<any> {
return hm
.reverseGeocode([coordinate.latitude, coordinate.longitude] as any)
.then((response) => {
return response;
});
}

- Chỉ đường:

repository:

```ts

public async route(
location: GeoCoordinates,
coordinate: GeoCoordinates,
): Promise<any> {
    return hm
    .route([location.latitude, location.longitude], [
    coordinate.latitude,
    coordinate.longitude,
    ] as any)
    .then((response) => {
    return response;
    });
}

```

- Lấy ra 1 mảng latlng sau đó kết hợp với <Polyline /> để vẽ ra tuyến đường từ điểm đầu tới điểm cuối
