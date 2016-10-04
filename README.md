## Connecting to Eduroam on Arch Linux

First of all you need to know what is your wireless interface. Use the command `ip link` to find it out. Wireless interfaces start with **wl**. The command `ip link | grep wl` should list all your interfaces. Use the command `ip link | grep wl | perl -n -e '/^\d:\s+(.*):/ && print $1')` to extract the name of your wireless interface. You need to make sure that you're connected to your interface by running the command `sudo ip link set interface_name up`.


After this you need to make sure that the **netctl** tool is installed. As this tool is part of the **base** group, it should already be installed in your system. After this we need to create a new profile in the folder `/etc/netctl/`. Create a new file in this folder named eduroam or whatever you want.


Copy these lines to that file:


```
Connection='wireless'
Interface='YOUR_INTERFACE'
Security='wpa-configsection'
Description="eduroam network"
IP='dhcp'
TimeoutWPA=30
WPAConfigSection=(
    'ssid="eduroam"'
    'key_mgmt=WPA-EAP'
    'eap=TTLS'
    'proto=WPA2'
    'phase2="auth=PAP"'
    'anonymous_identity="YOUR_IDENTITY"'
    'identity="YOUR_IDENTITY"'
    'password="YOUR_PASSWORD"'
)
```


After this we're almost done, just start that profile by running `sudo netctl start NAME_OF_YOUR_PROFILE`
