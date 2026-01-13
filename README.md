# VLAN-Automation


The python code below is the script that automates the creation of a VLAN according to the name you give it, number, and interfaces assigned to it:
```
from netmiko import ConnectHandler

vlan_name = input("Enter name of new VLAN: ")
vlan_number = input("Enter VLAN number (Ex: 10): ")
vlan_interfaces = input(
    "Enter interface names (g0/0,g2/1,etc.)\n"
    "Separate with commas, no spaces: "
)

interfaces = vlan_interfaces.split(",")

iosv_l2 = {
    'device_type': 'cisco_ios',
    'ip': '192.168.x.x',
    'username': 'John',
    'password': 'P@$$w0rd'
}

net_connect = ConnectHandler(**iosv_l2)

print("Configuring VLAN...")
net_connect.send_config_set([
    f"vlan {vlan_number}",
    f"name {vlan_name}"
])

print("Configuring Interfaces...")
interface_commands = []
for intf in interfaces:
    interface_commands.extend([
        f"interface {intf}",
        "switchport mode access",
        f"switchport access vlan {vlan_number}",
        "no shutdown"
    ])

net_connect.send_config_set(interface_commands)

print("Configuration Done.")
net_connect.disconnect()

```
