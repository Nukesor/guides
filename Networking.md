# Networking tools


## Tcpdump

- `tcdpump -i vpn` Filter on interface `vpn`.
- `tcdpump -i vpn src 10.1.0.1` Filter all devices FROM `10.1.0.1` on interface `vpn`.
- `tcdpump -i vpn dst 10.1.0.1` Filter all devices TO `10.1.0.1` on interface `vpn`.
