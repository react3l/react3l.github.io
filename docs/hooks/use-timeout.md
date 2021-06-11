---
layout: default
title: useTimeout
parent: Hooks
---

# Timeout Hook

{: .no_toc }

## Table of contents

{: .no_toc .text-delta }

1. TOC
   {:toc}

---

## Create and manage timeouts

Similar to [useInterval](../use-interval/), if you need a component that has an timeout during its lifecycle. `useTimeout` is created  for you.

For example: Mark something expired after `x` seconds

```tsx
function ExampleComponent() {
   const [expired, setExpired] = React.useState<boolean>(false);

   useInterval(
      () => {
         setExpired(true);
      },
      10000,
   );
}
```

You don't have to care about calling `clearTimeout`
