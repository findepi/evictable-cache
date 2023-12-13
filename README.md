# evictable-cache

Evictable caches are caches that support eviction (invalidation)
[correctly](https://github.com/google/guava/issues/1881).

# Usage

If you are already using Guava's `com.google.common.cache.CacheBuilder`,
just replace it with `EvictableCacheBuilder`. `EvictableCacheBuilder` is
designed to be a drop-in replacement, providing strong visibility guarantees
while hiding all the gory details behind Guava `Cache` and `LoadingCache`
interfaces you're already familiar with.

```java
Cache<String, Value> cache = EvictableCacheBuilder.newBuilder()
    .maximumSize(10_000)
    .expireAfterWrite(1, TimeUnit.HOURS)
    .build();
```
— or —

```java
LoadingCache<String, Value> loadingCache = EvictableCacheBuilder.newBuilder()
        .maximumSize(10_000)
        .expireAfterWrite(1, TimeUnit.HOURS)
        .build(CacheLoader.from((String key) -> ...));
```

# Installation

Get it from Maven Central

```xml
<dependency>
    <groupId>io.github.findepi</groupId>
    <artifactId>evictable-cache</artifactId>
    <version>1.0</version>
</dependency>
```

# Other Java cache implementations

- [Guava](https://github.com/google/guava) `Cache` (which this library is baseed on). Awesome if you do not need
  strong visibility guarantees after `invalidate()` (or when you do not need `invalidate()` at all),
  or when cache usage is not concurrent (multi-threaded).
- [Caffeine](https://github.com/ben-manes/caffeine) (recommended by Guava javadocs). It comes in two flavors: synchronous and async.
  - Synchronous is awesome if you don't mind that invalidation blocks waiting for ongoing loads to finish.
  - Async is awesome if you do not need strong visibility guarantees after `invalidate()`, or some infrequent
    race conditions are acceptable.
