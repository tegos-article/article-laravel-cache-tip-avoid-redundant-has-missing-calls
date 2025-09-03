# Laravel Cache Tip: Avoid Redundant has/missing Calls

![Laravel Cache Tip: Avoid Redundant has/missing Calls](assets/poster.jpg)

This article explains a common inefficiency in Laravel cache usage: calling `Cache::has()` or `Cache::missing()` immediately before `Cache::get()`. That pattern results in **two cache operations instead of one**, adding unnecessary overhead.

## Key Points

- ❌ **Inefficient**
  ```php
  if (Cache::has($key)) {
      return Cache::get($key);

  }
  ````

* ✅ **Optimized**

  ```php
  $value = Cache::get($key);
  if ($value !== null) {
      return $value;
  }
  ```

* ✅ Or use `Cache::remember()` when possible:

  ```php
  Cache::remember($key, 600, fn () => User::count());
  ```

## Benchmarks

| Pattern      | Time | Memory | Cache Ops |
|--------------|------|--------|-----------|
| With `has()` | 14ms | 47MB   | 2         |
| Without      | 6ms  | 43MB   | 1         |

👉 One less cache call per request = faster responses + lower memory usage.

## Read the Full Article

[Laravel Cache Tip: Avoid Redundant has/missing Calls](./1-article.md)