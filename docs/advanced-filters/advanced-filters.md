---
layout: default
title: Maps
nav_order: 9
---

# Advanced-Filters

## Installation

- Add .npmrc:

```ts
@react3l:registry=https://npm.pkg.github.com
```

- Note:

  Be careful with file .npmrc

## IdFilter

For primary key in integer format

```ts
export declare class IdFilter extends Filter {
  equal?: number

  notEqual?: number;

  in?: number[]

  notIn?: number[]
}
```

## GuidFilter

For primary key in GUID format

```ts
export declare class GuidFilter extends Filter {
  equal?: string

  notEqual?: string;

  in?: string[]

  notIn?: string[]
}
```

## DateFilter

For date/time fields

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

For number fields

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

## Note:

- In the project we can use together with listService.

## Example:

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
