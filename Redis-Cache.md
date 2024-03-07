[Redis](https://redis.io/docs/about/) is an open-source in-memory data store offering database, cache, and message broker functionalities. Redis is required as it allows for the following features to work:

- Persistent Browser Sessions: User sessions survive container restarts
- Asynchronous Scans: Scans run asynchronously, preventing main thread blocks
- Persistent IGDB Access Tokens: IGDB tokens persist across restarts

### Enabling Redis Support

To start using Redis, add these environment variables (see [docker-compose.example.yml](https://github.com/zurdi15/romm/blob/master/examples/docker-compose.example.yml))

```
- ENABLE_EXPERIMENTAL_REDIS=true
- REDIS_HOST=localhost # Redis server hostname or IP.
- REDIS_PORT=6379 # Redis server port number.
```