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

## Add an IP for SSh
To add an IP you need the vlan you just configured.

To configure the IP address do the folowing steps.
`Switch#`

```
conf t
```
`switch(config)#`

```
interface vlan "number"
```
`switch(config-if)#`

```
ip address 192.168.1.1 255.255.255.0
```

Go back into enable mode and type

```
show interface vlan <vlan number>
```
## Create Super Admin User

To log into the switch with ssh you need a super admin user.

`switch(config)#`
```
username "username" privilege 15 secret "password"
```

## Install SSH on switch

`switch(config)#`

```
ip ssh version 2
```

## Configure authentifiaction methode

`switch(config)#`

```
aaa new-model
```
```
aaa authentication login default local
```

### Add an enable password

`switch(config)#`
```
enable secret <password>
```
## Banners

Lets give our CLI some cool effects

>[!note]
>These "cool" effects are also here to warn unwanted people on the device the risk they are taking.

We are going to create a login password that will greet people when they type the ssh command or enter the rs232 cable interface.

`switch(config)#`
```
banner login @

                 ______________________________    ________         __________      ______  
_________  _________  __ )___  _/_  __ \_  ___/    __  ___/__      ____(_)_  /_________  /_ 
_  _ \_  |/_/  _ \_  __  |__  / _  / / /____ \     _____ \__ | /| / /_  /_  __/  ___/_  __ \
/  __/_>  < /  __/  /_/ /__/ /  / /_/ /____/ /     ____/ /__ |/ |/ /_  / / /_ / /__ _  / / /
\___//_/|_| \___//_____/ /___/  \____/ /____/      /____/ ____/|__/ /_/  \__/ \___/ /_/ /_/

____________________________________________________________________________________________

                Login with your correct username and correct password.
____________________________________________________________________________________________

@
```
>[!tip]
>You can do cool ASCII art by going on ASCII art generators and typing text like this [ASCII Art generator](https://patorjk.com/software/taag/#p=display&f=Graffiti&t=Type%20Something%20)

>[!warning]
>The Switch's CLI does **NOT** inlcude other characters then the printable ones. Basically all the letters and symbols that you will find on a basic american querty. You can also visite the followinf website which will indicate the [Printable Characters](https://www.ascii-code.com/characters/printable-characters#:~:text=ASCII%20printable%20characters%20are%20the,text%20and%20other%20visual%20content.)

Now lets make a banner that will appear when the user logs in !

`switch(config)#`
```
banner motd @
                                                (        )   (
                                            (   )\ )  ( /(   )\ )
                            (     )   (   ( )\ (()/(  )\()) (()/(
                           ))\ ( /(  ))\  )((_) /(_))((_)\   /(_))
                          /((_))\())/((_)((_)_ (_))    ((_) (_))
                         (_)) ((_)\(_))   | _ )|_ _|  / _ \ / __|
                         / -_)\ \ // -_)  | _ \ | |  | (_) |\__ \
                         \___|/_\_\\___|  |___/|___|  \___/ |___/

                  # # # # # # # # # # # # # # # # # # # # # # # # # # # #
                  #                                                     #
                  #                     WARNING                         #
                  #                                                     #
                  #     UNAUTHORIZED ACCESS IS STRICTLY PROHIBITED.     #
                  #                                                     #
                  #   ===============================================   #
                  #                                                     #
                  #   All activities on this network are monitored      #
                  #   and logged for security purposes. Unauthorized    #
                  #   access attempts or activities will be reported    #
                  #   to the appropriate authorities and may result     #
                  #   in disciplinary action, termination of access,    #
                  #   and/or legal proceedings.                         #
                  #                                                     #
                  # # # # # # # # # # # # # # # # # # # # # # # # # # # #
@
```

This combo can be awkward while loging in with a serial cable because both the login banner and motd banner will appear at the same tim but when you log in with ssh th login banner will appear then when the password is entered the motd banner will appear.
## Conclusion

Here is our configuration so far

```
sh run
```
```
Current configuration : 5364 bytes
!
version 12.2
no service pad
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname Switch
!
boot-start-marker
boot-end-marker
!
enable secret 5 $1$Y8Ry$6kXyBagbGqR3MlPhkOHvf0
!
username "username" privilege 15 secret 5 $1$dMKH$wT7zZ409nihK7lIxQKoS00
!
!
aaa new-model
!
!
aaa authentication login default local
!
!
!
aaa session-id common
switch 1 provision ws-c3750-24p
system mtu routing 1500
authentication mac-move permit
ip subnet-zero
!
!
!
!
crypto pki trustpoint TP-self-signed-7988608
 enrollment selfsigned
 subject-name cn=IOS-Self-Signed-Certificate-7988608
 revocation-check none
 rsakeypair TP-self-signed-7988608
!
!
crypto pki certificate chain TP-self-signed-7988608
 certificate self-signed 01
  30820239 308201A2 A0030201 02020101 300D0609 2A864886 F70D0101 04050030
  2E312C30 2A060355 04031323 494F532D 53656C66 2D536967 6E65642D 43657274
  69666963 6174652D 37393838 36303830 1E170D39 33303330 31303030 3130375A
  170D3230 30313031 30303030 30305A30 2E312C30 2A060355 04031323 494F532D
  53656C66 2D536967 6E65642D 43657274 69666963 6174652D 37393838 36303830
  819F300D 06092A86 4886F70D 01010105 0003818D 00308189 02818100 9D8C74C4
  3464517F BA763F12 FCDD52E3 E217E1E9 54D41500 948960EE 247CC384 275CAF09
  0614CB87 953F42FE 6883343B F2A88C77 E511725C 9A091127 CD1DAB68 F634135F
  923AB162 8D2748B0 2B9A6CA0 B48A7BB9 ED9B66AB 009622D0 DBF12A27 0BD61992
  796DA86F C1B1C3CB 7A86987F 5564F4EB 4ECD924A 075E9314 66F80399 02030100
  01A36730 65300F06 03551D13 0101FF04 05300301 01FF3012 0603551D 11040B30
  09820753 77697463 682E301F 0603551D 23041830 168014D3 3676901F FF851643
  971849D2 1F4A7571 040A3D30 1D060355 1D0E0416 0414D336 76901FFF 85164397
  1849D21F 4A757104 0A3D300D 06092A86 4886F70D 01010405 00038181 0078D332
  4AA22611 4C4E6B78 7C988BB9 A354E5D8 F32A576B 94257329 48EC90F0 673D9483
  B5D3572C 4B86B5FD CF6AC458 B7097C4C 855F21E7 E56845FD C8D4CB87 020443DE
  9396D9B1 E8C185C0 0F3AB926 6DAAB52D 704CC313 A971C71B 8D3A2E06 AEA63864
  6CCD03B3 871D45FB 15152FA5 CFF48ECF 82650FBC CEE8A066 4878C210 32
  quit
!
!
!
spanning-tree mode pvst
spanning-tree etherchannel guard misconfig
spanning-tree extend system-id
!
vlan internal allocation policy ascending
!
ip ssh version 2
!
!
interface FastEthernet1/0/1
!
interface FastEthernet1/0/2
 switchport access vlan 42
!
interface FastEthernet1/0/3
!
interface FastEthernet1/0/4
!
interface FastEthernet1/0/5
!
interface FastEthernet1/0/6
!
interface FastEthernet1/0/7
!
interface FastEthernet1/0/8
!
interface FastEthernet1/0/9
!
interface FastEthernet1/0/10
!
interface FastEthernet1/0/11
!
interface FastEthernet1/0/12
!
interface FastEthernet1/0/13
!
interface FastEthernet1/0/14
!
interface FastEthernet1/0/15
!
interface FastEthernet1/0/16
!
interface FastEthernet1/0/17
!
interface FastEthernet1/0/18
!
interface FastEthernet1/0/19
!
interface FastEthernet1/0/20
!
interface FastEthernet1/0/21
!
interface FastEthernet1/0/22
!
interface FastEthernet1/0/23
!
interface FastEthernet1/0/24
!
interface GigabitEthernet1/0/1
!
interface GigabitEthernet1/0/2
!
interface Vlan1
 no ip address
!
interface Vlan42
 ip address 192.168.1.200 255.255.255.0
!
ip classless
ip http server
ip http secure-server
!
ip sla enable reaction-alerts
!
banner login ^C
                 ______________________________    ________         __________      ______
_________  _________  __ )___  _/_  __ \_  ___/    __  ___/__      ____(_)_  /_________  /_
_  _ \_  |/_/  _ \_  __  |__  / _  / / /____ \     _____ \__ | /| / /_  /_  __/  ___/_  __ \
/  __/_>  < /  __/  /_/ /__/ /  / /_/ /____/ /     ____/ /__ |/ |/ /_  / / /_ / /__ _  / / /
\___//_/|_| \___//_____/ /___/  \____/ /____/      /____/ ____/|__/ /_/  \__/ \___/ /_/ /_/
____________________________________________________________________________________________
                Login with your correct username and correct password.
____________________________________________________________________________________________
^C
banner motd ^C
                                                (        )   (
                                            (   )\ )  ( /(   )\ )
                            (     )   (   ( )\ (()/(  )\()) (()/(
                           ))\ ( /(  ))\  )((_) /(_))((_)\   /(_))
                          /((_))\())/((_)((_)_ (_))    ((_) (_))
                         (_)) ((_)\(_))   | _ )|_ _|  / _ \ / __|
                         / -_)\ \ // -_)  | _ \ | |  | (_) |\__ \
                         \___|/_\_\\___|  |___/|___|  \___/ |___/

                  # # # # # # # # # # # # # # # # # # # # # # # # # # # #
                  #                                                     #
                  #                     WARNING                         #
                  #                                                     #
                  #     UNAUTHORIZED ACCESS IS STRICTLY PROHIBITED.     #
                  #                                                     #
                  #   ===============================================   #
                  #                                                     #
                  #   All activities on this network are monitored      #
                  #   and logged for security purposes. Unauthorized    #
                  #   access attempts or activities will be reported    #
                  #   to the appropriate authorities and may result     #
                  #   in disciplinary action, termination of access,    #
                  #   and/or legal proceedings.                         #
                  #                                                     #
                  # # # # # # # # # # # # # # # # # # # # # # # # # # # #
^C
!
line con 0
line vty 5 15
!
end
```
