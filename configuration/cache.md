# Configuring Omnixent Cache

Omnixent cache can be configured via environment variables exclusively.
Omnixent only supports **Redis** as a caching layer at the time of writing, but there's a community effort to make the cache more configurable, enabling other sources.

- **Enable cache** \
`REDIS_ENABLED=true`
- **Redis connection string** \
`REDIS_URL=redis://my-redis-url:6379`
- **Redis cache expiration time** (in seconds, optional, default is `86400000`) \
`DEFAULT_CACHE_EXPIRE_TIME=86400000`