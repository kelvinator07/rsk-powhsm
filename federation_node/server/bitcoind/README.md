## BUILD
`docker build -t bitcoind .`

## RUN
```sh
docker run -d --name bitcoind01  -p 18333:18333 -p 18332:18332 -v $(pwd)/bitcoind:/root/.bitcoin bitcoind
```
