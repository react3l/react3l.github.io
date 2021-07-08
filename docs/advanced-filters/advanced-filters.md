---
layout: default
title: Maps
nav_order: 9
---

# Advanced-Filters

## Advanced-Filters là gì?

- Đây là 1 phần của React3l.

- Có thể hiểu Advanced-Filters là nâng cao của dạng filter thông thường. Bao gồm 5 loại chính: IdFilter, GuidFilter, DateFilter, NumberFilter, StringFilter.

## Cài đặt

npm

```sh
npm install @react3l/advanced-filters
```

yarn

```sh
yarn @react3l/advanced-filters
```

- Tạo file .npmrc và cấu hình:

* Lưu ý nếu xảy ra lỗi 401 thì cần phải tạo token trên github và login bằng lệnh sau:

```sh
npm login --registry=https://npm.pkg.github.com
```

```sh
@react3l:registry=https://npm.pkg.github.com
```

## Một số khái niệm

- equal: So sánh bằng.

- notEqual: So sánh không bằng.

- in: Lấy các phần tử nằm trong mảng.

- notIn: Lấy các phần tử ngoài mảng.

- greater: Lớn hơn.

- greaterEqual: Lớn hơn hoặc bằng.

- less: Nhở hơn.

- lessEqual: Nhỏ hơn bằng bằng.

- contain: Chứa (Thường dùng với StringFilter để biểu thị việc lấy tất cả dữ liệu có chứa được filter).

- notContain: Không chứa (ngược lại với contain).

- startWith: Bắt đầu với.

- endWith: Kết thúc với.

## IdFilter

Dành cho primary key với kiểu integer.

```ts
export declare class IdFilter extends Filter {
  equal?: number

  notEqual?: number;

  in?: number[]

  notIn?: number[]
}
```

## GuidFilter

```ts
export declare class GuidFilter extends Filter {
  equal?: string

  notEqual?: string;

  in?: string[]

  notIn?: string[]
}
```

## DateFilter

Dành cho dữ liệu kiểu date/time

```tsx
import { Moment } from "moment"

export declare class DateFilter extends Filter {
  equal?: Moment

  notEqual?: Moment

  greater?: Moment

  greaterEqual?: Moment

  less?: Moment

  lessEqual?: Moment
}
```

## NumberFilter

Dành cho dữ liệu kiểu number

```ts
import { Filter, FilterType } from "Filter"

export declare class NumberFilter extends Filter {
  equal?: number

  notEqual?: number

  greater?: number

  greaterEqual?: number

  less?: number

  lessEqual?: number
}
```

## StringFilter

Dành cho dữ liệu kiểu string

```tsx
import { Filter, FilterType } from "Filter"
export declare class StringFilter extends Filter {
  startWith?: string

  notStartWith?: string

  endWith?: string

  notEndWith?: string

  equal?: string

  notEqual?: string

  contain?: string

  notContain?: string
}
```

## Example:

Ví dụ này sử dụng kèm với listService để lấy ra danh sách dữ liệu có sử dụng filter.

```ts
import {
  DateFilter,
  IdFilter,
  NumberFilter,
  StringFilter,
} from "@react3l/advanced-filters"
import { ModelFilter } from "@react3l/react3l/core/model-filter"

export class Type_Target_Filter extends ModelFilter {
  public id?: IdFilter = new IdFilter()

  public code?: StringFilter = new StringFilter()

  public search?: string
}
```

```ts
const [
  arrayList,
  total,
  loading,
  refreshing,
  filter,
  handleLoadMore,
  handleRefresh,
  handleSearch,
  dispatch,
  isFirstLoading,
] = listService.useList<Type_Target, Type_Target_Filter, "search">(
  IndirectSalesOrderFilter,
  API_LIST,
  API_COUNT,
  "search"
)
```

```ts
const action: ListAction<Type_Target, Type_Target_Filter, "search"> = {
  type: ACTION_RESET_FILTER,
  newFilter: {
    ...new Type_Target_Filter(),

    orderDate: {
      greaterEqual: moment().startOf("day"),
      lessEqual: moment().endOf("day"),
    },
  },
}
dispatch(action)
```
