```sh
docker run -d --name fednode01  -p 5050:5050 -p 4444:4444 -p2226:22 -v $(pwd)/config:/etc/rsk -v $(pwd)/database:/var/lib/rsk/ -v $(pwd)/logs:/var/log/rsk --link hsm-server --link bitcoind01 fednode
```