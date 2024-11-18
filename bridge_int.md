# Steps to create a bridge interface and link it to the physical interface

## Create a new bridge interface
```bash
sudo ip link add br0 type bridge  
```
This creates a new bridge interface with name 'br0'. Create the same number of bridge interfaces as physical intefaces available on the server.

## Create /etc/netplan/01-kvmbridge.yaml
```yaml
network:
  ethernets:
    interface_name:
      dhcp4: false
  bridges:
    br0:
      interfaces: [interface_name]
      dhcp4: true
      mtu: 1500
      parameters:
        stp: true
        forward-delay: 15

```
Add your physical interface name in the place of `interface_name`.

## Save this file and follow the below to link the physical and bridge interface.
1. Run `sudo netplan try` to check for errors and format of the file.
2. Resolve errors if any.
3. Run `sudo netplan apply` to apply the settings and reboot the server. ***You will need physical access to the machine as this applying the settings will disconnect the SSH session.*** 
