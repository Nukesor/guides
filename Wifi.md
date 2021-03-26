# Networking


## Wifi debugging

- `cat /proc/net/wireless` Show wlan status and signal levels.

## Iwctl

- `station $interface scan` Start looking for access points (AP)
- `station $interface get-networks` Show all APs that are found while scanning.
- `station $interface connect $AP` Connect to AP by ESSID.
        Surround with `$AP` `'` if there are any special characters.
- `station $interface show` Show current state of interface

## Monitor mode

```
sudo ip link set $device down
sudo iwconfig $device mode monitor
sudo ip link set $device up
```

To go back to normal (managed) mode, replace `monitor` with `managed`.

## Channel

- `iwlist $device channel` Show the current channel of the device.
- `sudo iwconfig $device channel $channel` Switch device to channel.
