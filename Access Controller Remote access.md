## Manual Remote access
A new feature added to the access controller is the ability to remotely open the door via SSH.
Once you have gained access via SSH, you can enter the access controller directory with  
`cd AccessController`  
Then to open the door remotely you can type:  
`python manual_access.py open`  
Which will open the door.  

To later arm the alarm you can type:  
`python manual_access.py arm_alarm`  

## Remote access via Raspberry Pi Connect
This is the preferred access mode. It uses the Hackerspace Raspberry Pi Connect account to provide connectivity from anywhere. If you need access to this, contact a Committee member.

## Remote access via Tailscale
Tailscale is a VPN software which you can access networks remotely with a simple user interface.
With Tailscale installed on your system you can connect to the access controller.
To access to the Tailscale you can find the link in the Bitwarden account under "Access Controller Tailscale"

## Remote access via Secure Shell (`ssh`)
The Pi has port `22` open for `ssh` access from within the LAN. 

If you have access to a \*ix command line you can access it via:

``` bash
# Within the LAN at the Space
ssh -p 22 pi@access-controller.local
```

As usual, the password is in the BitWarden vault.