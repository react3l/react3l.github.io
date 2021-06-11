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

Managing "loading" state of a component is very common task in React projects. For every state in every component, you have to call the `useState` hook, then create from 1 to 3 callbacks to change the state.
See the example code:

```tsx
function ExampleComponent() {
  const [loading, setLoading] = React.useState<boolean>(false);

  const handleToggleState = React.useCallback(() => {
    setLoading(!loading);
  }, [loading]);
  // ...
}
```

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

## Example
The `isVisible` state of a modal

```tsx
const [
  isVisible, 
  toggleModal,
  openModal,
  closeModal,
] = useBoolean(false);
```
