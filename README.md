# Laravel Cache Tip: Avoid Redundant `has` / `missing` Calls

![Laravel Cache Tip: Avoid Redundant has/missing Calls](assets/poster.jpg)

Calling `Cache::has()` or `Cache::missing()` immediately before `Cache::get()` in Laravel is **inefficient**: it results in **two cache operations instead of one**, adding unnecessary overhead.

This guide shows how to optimize your cache usage.

---

## ❌ Inefficient Example

```php
if (Cache::has($key)) {
    return Cache::get($key);
}
````

## ✅ Optimized Example

```php
$value = Cache::get($key);
if ($value !== null) {
    return $value;
}
```

## ✅ Use `Cache::remember()` When Possible

```php
Cache::remember($key, 600, fn() => User::count());
```

---

## Benchmarks

| Pattern      | Time | Memory | Cache Ops |
|--------------|------|--------|-----------|
| With `has()` | 14ms | 47MB   | 2         |
| Without      | 6ms  | 43MB   | 1         |

> One less cache call per request = faster responses + lower memory usage.

## 📎 Read the Full Article

[Laravel Cache Tip: Avoid Redundant has/missing Calls](https://dev.to/tegos/laravel-cache-tip-avoid-redundant-hasmissing-calls-4hi1)
