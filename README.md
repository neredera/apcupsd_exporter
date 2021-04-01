# apcupsd_exporter [![Linux Test Status](https://github.com/mdlayher/apcupsd_exporter/workflows/Linux%20Test/badge.svg)](https://github.com/mdlayher/apcupsd_exporter/actions)  [![GoDoc](http://godoc.org/github.com/mdlayher/apcupsd_exporter?status.svg)](http://godoc.org/github.com/mdlayher/apcupsd_exporter)

Command `apcupsd_exporter` provides a Prometheus exporter for the
[apcupsd](http://www.apcupsd.org/) Network Information Server (NIS). MIT
Licensed.

## Usage

Available flags for `apcupsd_exporter` include:

```
$ ./apcupsd_exporter -h
Usage of ./apcupsd_exporter:
  -apcupsd.addr string
        address of apcupsd Network Information Server (NIS) (default ":3551")
  -apcupsd.network string
        network of apcupsd Network Information Server (NIS): typically "tcp", "tcp4", or "tcp6" (default "tcp")
  -telemetry.addr string
        address for apcupsd exporter (default ":9162")
  -telemetry.path string
        URL path for surfacing collected metrics (default "/metrics")
```

## Sample Docker Usage

apcupsd has to be accessible from the docker container.
It can be running on docker as well, e.g. [gersilex/apcupsd-docker](https://github.com/gersilex/apcupsd-docker)

Example `docker-compose.yml`:
```
version: '3.4'

services:
  apcupsd_exporter:
    image: neredera/apcupsd_exporter:latest
    restart: always
    command: "-apcupsd.addr apcupsd:3551"
    ports:
      - 9162:9162
    networks:
      - apcupsd
      - prometheus

  apcupsd:
    image: gersilex/apcupsd:v1
    restart: always
    tty: true
    devices:
      - /dev/usb/hiddev0:/dev/usb/hiddev0
    volumes:
      - ./apcupsd.conf:/etc/apcupsd/apcupsd.conf
    ports:
      - 3551:3551
    networks:
      - apcupsd

networks:
  apcupsd:
  prometheus: # prometheus network (for access to apcupsd_exporter).
    external:
      name: prometheus
    
```
