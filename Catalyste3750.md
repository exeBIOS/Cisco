# Cisco Configuration
Here is my lab switch, i test configurations on it.
## Information

>[!caution]
>If for any reason you need to **RESET FACTORY** the switch be **CAUTIOUS**, some commands can erase the IOS of the switch and delete all the content of any device connected to USB on the Switch.
>
>To fix this you can download the correct IOS file for your switch on the [Cisco Software Versions](https://software.cisco.com/research/home?pid=&sid=&cr=) and boot it back on the switch.
>
>The switch used for this demonstration does not have a USB port.

>[!important]
>When a word is between quotes like this: "example" dont implement the quotes see this as a variable.
>
>Example:
>
>Create a vlan:
>
>vlan "name"
>
>Dont do this:
>
>vlan "exebios"
>
>Do this:
>
>vlan exebios

>[!tip]
>This CLI has a lot of short cuts just like a regular cmd or linuc termianl you can press `tab` to complete the command faster.
>
>Some words can also be shortened like fore enable mode dont type **enable** just **en** and **configure terminal** type **conf t**
## Process
Enter **Enable Mode**

`Switch>`
```
en
```
`Switch#`

Enter **Configuration Mode**
```
conf t
```
`Switch(config)#`

We are going to configure a **Vlan** for port number 2 a.k.a 1/0/2.

## Create the **Vlan**

```
Vlan "number"
```

Let's change the Vlan's name, first go into vlan configuration mode.

`Switch(config)#`
```
vlan "number"
```

`Switch(config-vlan)#`

Then change the vlan's name

```
vlan "number" "name"
```
## Attribute the vlan to a port
Now let's attribute the vlan to port number 2 a.k.a 1/0/2

Go back into configuration mode

`Switch(config-vlan)#`
```
exit
```
`Switch(config)#`

Access the Interface 1/0/2
```
Interface FastEthernet 1/0/2
```
`Switch(config-if)#`

Attribute the vlan you created with the port 1/0/2

```
switchport access vlan42
```

Go back into enable mode

```
end
```
`Switch#`

To see if the configuration worked type

```
show vlan
```
You should be greeted with these informations

```
VLAN Name                             Status    Ports

---- -------------------------------- --------- -------------------------------

1    default                          active    Fa1/0/1, Fa1/0/3, Fa1/0/4

                                                Fa1/0/5, Fa1/0/6, Fa1/0/7

                                                Fa1/0/8, Fa1/0/9, Fa1/0/10

                                                Fa1/0/11, Fa1/0/12, Fa1/0/13

                                                Fa1/0/14, Fa1/0/15, Fa1/0/16

                                                Fa1/0/17, Fa1/0/18, Fa1/0/19

                                                Fa1/0/20, Fa1/0/21, Fa1/0/22

                                                Fa1/0/23, Fa1/0/24, Gi1/0/1

                                                Gi1/0/2

"vlan number"   "vlan name"                            active    Fa1/0/2
```
