---
layout: default
title: useInterval
parent: Hooks
---

# Interval Hook

{: .no_toc }

## Table of contents

{: .no_toc .text-delta }

1. TOC
   {:toc}

---

## Create and manage intervals

Sometimes, you may need a component that has an interval loop during its lifecycle. `useInterval` is created  for you.

For example: increasing counter

```tsx
function ExampleComponent() {
   const [counter, dispatch] = React.useReducer<Reducer<number, 'increase' | 'decrease'>>(
      counterReducer,
      0,
      () => 0,
   );

   useInterval(
      () => {
         dispatch('increase');
      },
      1000
   );
}
```

You don't have to care about calling `clearInterval`