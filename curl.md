# Networking
[<TIL](Programming.md)
## Authorization header missing in PHP POST request

Add the following lines to _.htaccess_ file

```
RewriteCond %{HTTP:Authorization} ^(.*)
RewriteRule .* - [e=HTTP_AUTHORIZATION:%1]
```
[src](https://stackoverflow.com/a/26791450/174320)

## Determine the IP of a domain
`$ host domainname.com`

or more detailed info by using:

`$ dig domainname.com`

[src](https://github.com/jbranchaud/til/blob/master/devops/determine-the-ip-address-of-a-domain.md)

## How to make a POST request with :curl: from :command line:
`curl --data "param1=value1&param2=value2" https://example.com/resource.cgi`

or

`curl --data "param1=value1" --data "param2=value2" https://example.com/resource.cgi`

Without data:

`curl --data "" https://example.com/resource.cgi`

or

`curl --request POST https://example.com/resource.cgi`

## Check if a port is in use
The `lsof` command is used to _list open files_.
`$ lsof -i TCP:3000`

[source](https://github.com/jbranchaud/til/blob/master/unix/check-if-a-port-is-in-use.md)

## Multiple ssh keys

1. Check if there is a .ssh directory in the home root:

`mkdir ~/.ssh`

2. generate a ssh key (on the question about the passphrase just press ENTER)

`ssh-keygen -t rsa -b 4096`

3. copy the ida_rsa.pub file to the authorized_keys of the remote server

`cd ~/.ssh`
`scp -P 2222 id_rsa.pub user@domain.net:.ssh/authorized_keys`

or  the more simple solution:

`ssh-copy-id -i ~/.ssh/id_rsa.pub user@domain`

4. rename id_rsa to domain_rsa

`mv id_rsa domain_rsa`

5. add a shortcut in the .ssh/conf file

```
Host domainName
Hostname domain.net
IdentityFile ~/.ssh/domain_rsa
User www
```

_"User" is the username for logging over ssh_

## Make heavy use of the .ssh/config file
```
   Host x
       Hostname full.host.name.com  (or 1.2.3.4)
       User <myuser>
       IdentitiesOnly yes
       IdentityFile ~/.ssh/id_x_ed25519
```
Give hosts short names so you can `ssh x`

[Source](https://news.ycombinator.com/user?id=m463)

Use `Host *` at the beginning of the  config file for global settings
```
    Host *
        Ciphers aes128-ctr
        Compression yes
        ServerAliveInterval 120
        ForwardX11 yes
```
With this setup, typing `ssh example` is equivalent to
`ssh -XCY -c aes128-ctr my_name@example.url.com`
which definitely saves some keystrokes.

[Source](https://news.ycombinator.com/item?id=23027786)
## Automatic login
Generate identities for some machines

`  ssh-keygen -t ed25519 -f ~/.ssh/id_x_ed25519`

use ssh-copy-id to copy the identity to the target machine so it lets you in:

 ` ssh-copy-id -i ~/.ssh/id_x_ed25519.pub x`

or if your machine doesn't have ssh-copy-id:

  `cat ~/.ssh/id_x_ed25519.pub | ssh x "cat >> .ssh/authorized_keys"`

[Source](https://news.ycombinator.com/user?id=m463)

## Kill the ssh session
`~.`
Help about the ssh escape sequence:
`~?`

[Source](https://smallstep.com/blog/ssh-tricks-and-tips/)

## Exit automatically on network interruptions
In your .ssh/config, add:
```
ServerAliveInterval 5
ServerAliveCountMax 1
```
What happens is that ssh will check the connection by sending an echo to the
remote host every `ServerAliveInterval` seconds. If more than `ServerAliveCountMax`
echos are sent without a response, ssh will timeout and exit.

[Source](https://smallstep.com/blog/ssh-tricks-and-tips/)

## Use ssh server as a proxy to another SSH server
Useful for accessing servers behind a firewall, or using your own server as a proxy to
bypass a bottleneck in the network.

`$ ssh -J user1@host1 user_final@host_final`

[Source](https://news.ycombinator.com/item?id=23026196)

## Accessing internal resources externally via ssh

`ssh -D9090 user@remote`

Then, in Firefox, _set it to use a SOCK5 proxy of localhost:9090_ and _"Proxy DNS when using SOCKS v5"._
Now, when you use Firefox it is as if you are using Firefox on the machine you are SSH'd into(including DNS resolution!).
This is really handy for things like accessing otherwise unreachable resources or other internal resources externally.
It is also handy to be able to put all your web traffic as originating from a remote VPS with no advanced setup required.

[Source]( https://news.ycombinator.com/item?id=23027447 )

## Closing/opening ports:
Check the status of all ports with:

`sudo netstat -lnp`

Close and/or open ports with:

```
sudo ufw allow 22

sudo ufw deny 22
```

[src](https://askubuntu.com/questions/410218/how-to-close-an-open-port-in-ubuntu)

## Client-Bridge configuration for router
* Go FIRST to wireless, wireless security and enter the security type and key that matches your primary router.
* Hit SAVE, (or APPLY if you wish to)
* Go to the wireless, basic settings page and change the wireless mode to Client Bridge.
* Your wireless network mode, channel and encryption should be set to the same as the primary router.
* If you are using N only or Mixed with N, set your wireless channel width to match your primary
* Set the wireless network name to exactly the same as your primary router. Make sure spelling and capitalization match.
* Hit SAVE at the bottom. Then hit APPLY.
* Check to make sure all the configurations, including the mode, saved and the mode is still client bridge. If any changed, fix them, and save again.
* Goto Setup, basic setup and enter a router Local IP address of 192.168.1.6. (Presuming your primary router is on the 192.168.1.x subnet. Client bridge must match the subent of the primary router) Leave subnet mask at 255.255.255.0.
* Set the Gateway IP to your primary router. (Likely 192.168.1.1)
* Leave Local DNS blank (Some posts say you need to set local DNS to the same as the gateway IP so if you cannot access the internet, put the gateway in this section)
* Check Assign Wan port to a switch, if you wish to.
* Change your timezone and DST to match where you are.
* Hit Apply.
* Set your browser to 192.168.1.6 (or 192.168.A.6 with A=0 or whatever, if your subnet is different) and login to your router
* Goto Security, Firewall. Under Block Wan Requests, uncheck everything but "Filter Multicast" (Leave Filter multicast checked).
* Hit Save
* Disable SPI Firewall
* Hit Apply.
* Go to setup/advanced routing and change the operating mode from "gateway" to router. Hit Apply.
* Click "add" to add a Virtual Interface
* Enter a SSID
* Go to Wireless > Wireless Security and set the password

# List all apps that use the internet connection at the moment

`lsof -P -i -n`

[src](https://www.commandlinefu.com/commands/view/3542/show-apps-that-use-internet-connection-at-the-moment.-multi-language)
