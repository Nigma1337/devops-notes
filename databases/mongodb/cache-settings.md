Your mongodb might be running sorta slowly, this could be because wiredTigers cache is kinda small


```
db.serverStatus().wiredTiger.cache["maximum bytes configured"]/1024/1024/1024
db.adminCommand( { "setParameter": 1, "wiredTigerEngineRuntimeConfig": "cache_size=10G"})
db.serverStatus().wiredTiger.cache["maximum bytes configured"]/1024/1024/1024
```
these commands up the cache size to 10G.
