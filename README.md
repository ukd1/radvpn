## Decentralized VPN
[![Build Status](https://travis-ci.org/mehrdadrad/radvpn.svg?branch=master)](https://travis-ci.org/mehrdadrad/radvpn) 
[![Go Report Card](https://goreportcard.com/badge/github.com/mehrdadrad/radvpn)](https://goreportcard.com/report/github.com/mehrdadrad/radvpn)
[![GoDoc](https://godoc.org/github.com/mehrdadrad/radvpn?status.svg)](https://godoc.org/github.com/mehrdadrad/radvpn)

![Alt text](/docs/imgs/radvpn.png?raw=true "radvpn")

## Build
Given that the Go Language compiler (version 1.11 or greater is required) is installed, you can build it with:
```
go get https://github.com/mehrdadrad/radvpn
cd $GOPATH/src/github.com/mehrdadrad/radvpn
go build .
```

## Docker
```
docker run --privileged -d -p 8085:8085 -v $(pwd)/radvpn.yaml:/etc/radvpn.yaml -e RADVPN_NODE_NAME=node1 radvp
```

## Basic Config
With the default it tries to load config.yaml file indivitually at each node but can be configured to use same configuration through [etcd](https://github.com/etcd-io/etcd). once the configuration changed, it loads and applies new changes by itself. the below yaml is a simple configuration.

![Alt text](/docs/imgs/simpleconfig.png?raw=true "radvpn")

```yaml
revision: 1

crypto:
  type: gcm
  key: 6368616e676520746869732070617373776f726420746f206120736563726574

nodes:
  - node:
      name: node1
      address: 8.121.55.10
      privateAddresses:
        - 10.0.1.1/24
      privateSubnets:
        - 10.0.1.0/24
  - node:
      name: node2
      address: 84.12.92.45
      privateAddresses:
        - 10.0.2.1/24
      privateSubnets:
        - 10.0.2.0/24        
```
### Run
```
#radvpn -config radvpn.conf 
```

### Configuration keys
- revision - the watcher works based on the revision number once it increased, the configuration will be loaded immediatly
- server
  - keepalive - frequency duration of radvpn-to-radvpn ping to check if a connection is alive (default is 10 seconds)
  - insecure - disable /enable encryption (default is false)
  - mtu - sets the mtu of the tunnel interface
  - maxworkers - sets number of concurrent workers (read/write to/from tunnel concurrently) 
  - address - sets ip address and ports (format : ip:port)
  - name - sets the name of the current node 
- crypto
  - type
     - gcm - galois/counter mode
     - cbc - cipher block chaining
  - key - secret key
- etcd
  - endpoints - sets the etcd endpoints
  - timeout - sets etcd endpoints timeout
- nodes
  - node
     - name - node's name 
     - address - node's external ip address
     - privateAddresses - sets private address(es) on the tunnel interface
     - privateSubnets - sets reachable subnet(s) from currect node


### Configuration with [etcd](https://github.com/etcd-io/etcd)
![Alt text](/docs/imgs/radvpnetcd.png?raw=true "radvpn etcd")

[sample configuration](https://github.com/mehrdadrad/radvpn/blob/master/radvpn.yaml)
#### Run with etcd
```
#radvpn -config radvpn.conf -etcd
```
#### Update etcd from yaml file
```
#radvpn -update etcd -config radvpn.yaml
```

## License
This project is licensed under MIT license. Please read the LICENSE file.

## Contribute
Welcomes any kind of contribution, please follow the next steps:

- Fork the project on github.com.
- Create a new branch.
- Commit changes to the new branch.
- Send a pull request.

