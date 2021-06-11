---
layout: default
title: useEffectSubscription
parent: Hooks
---

# useEffectSubscription

{: .no_toc }

## Table of contents

{: .no_toc .text-delta }

1. TOC
   {:toc}

---

## Purpose

If you subscribe to an observable in React component lifecycle, you have to cleanup the subscription when component unmount.

`useEffectSubscription` handle the job for you.

## Usage

### Using pure effect

```ts
import { Observable, Subscription } from "rxjs"

function getUsers(): Observable<User[]> {
  return new Observable((subscriber) => {
    subscriber.next([])
  })
}

function useCustomHook() {
  React.useEffect(() => {
    const subscription: Subscription = getUsers().subscribe((users: User[]) => {
      // Do something
    })

    return function cleanup() {
      subscription.unsubscribe()
    }
  }, [])
}
```

### Using useEffectSubscription

```ts
import { useEffectSubscription } from "react3l-common"
import { Observable, Subscription } from "rxjs"

function getUsers(): Observable<User[]> {
  return new Observable((subscriber) => {
    subscriber.next([])
  })
}

function useCustomHook() {
  useEffectSubscription(getUsers)
}
```
