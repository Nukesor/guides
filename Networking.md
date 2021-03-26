# Networking tools


## Tcpdump

- `tcdpump -i vpn` Filter on interface `vpn`.
- `tcdpump -i vpn src 10.1.0.1` Filter all devices FROM `10.1.0.1` on interface `vpn`.
- `tcdpump -i vpn dst 10.1.0.1` Filter all devices TO `10.1.0.1` on interface `vpn`.

## Open connections

- `netstat --tcp --udp --listening --program --numeric (-tulpn)`
    * `--tcp` Get tcp connections
    * `--udp` Get udp connections
    * `--listening` Only show listening connections
    * `--program` Show program name and PID the socket belongs to.
    * `--numeric` Show numeric addresses (e.g. `127.0.0.1` instead of `localhost`)
