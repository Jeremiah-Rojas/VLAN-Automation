## Network Topology
<img width="824" height="491" alt="image" src="https://github.com/user-attachments/assets/d4792172-149c-460e-86ea-f2330db68d3f" />


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
### Configurations in Action
This is the script running and the values entered in:
</br><img width="550" height="328" alt="image" src="https://github.com/user-attachments/assets/007586c4-3ba0-42fa-a957-fea4d10cb07f" />
This image shows the configurations that were performed by the script:
<br><img width="550" height="328" alt="image" src="https://github.com/user-attachments/assets/a4e0af81-a053-4bef-8efd-676c99b79c3b" />



