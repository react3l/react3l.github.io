---
layout: default
title: useBoolean
parent: Hooks
---

# Hook: useBoolean

{: .no_toc }

## Table of contents

{: .no_toc .text-delta }

1. TOC
   {:toc}

---

## Purpose

`useBoolean` is implemented to help you handle boolean state easily without writing callbacks.

## Usage

```ts
import { useBoolean } from "react3l-common"

function useCustomHook() {
  const [
    // boolean value
    boolValue,
    // toggle state
    toggle,
    // set state to true
    setTrue,
    // set state to false
    setFalse,
  ] = useBoolean(false)
}
```
