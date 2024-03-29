# Cisco Configuration
Here is my lab switch, i test configurations on it.
## Conf

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

Create the **Vlan**

```
Vlan "number"
```

Let's change the Vlan's name, first go into vlan configuration mode.

`Switch(config)#`
```
vlan "number"
```

`Switch(config-vlan)`

Then change the vlan's name

```
vlan "number" "name"
```

Now let's attribute the vlan to port number 2 a.k.a 1/0/2

Go back into configuration mode

```
end
```

Access the Interface 1/0/2
```
Interface FastEthernet1/0/2
```
`Switch(config-if)#`
