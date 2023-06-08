## Redis command cheat-sheet
- Start redis-server, save each 20 minutes, set log level, disable password
```sh
redis-server --save 20 1 --loglevel warning --requirepass "" 
```
- Connect to remote database on specific port with password
```sh
redis-cli -h redis15.localnet.org -p 6390 -a mysecret PING
```
(**Note**: password also can be provided automatically through `REDISCLI_AUTH` env variable)
- Connect to local redis-server
```sh
redis-cli
```