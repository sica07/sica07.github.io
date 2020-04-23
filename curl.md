
# Networking
[<TIL](Programming.md)
- [Determine the IP of a domain](#Determine the IP of a domain)
- [How to make a POST request with :curl: from :command line:](#How to make a POST request with :curl: from :command line:)
- [Check if a port is in use](#Check if a port is in use)
- [Multiple ssh keys](#Multiple ssh keys)

## Determine the IP of a domain
`$ host domainname.com`

or more detailed info by using:

`$ dig domainname.com`

[inspiration](https://github.com/jbranchaud/til/blob/master/devops/determine-the-ip-address-of-a-domain.md)

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

4. rename id_rsa to domain_rsa

`mv id_rsa domain_rsa`

5. add a shortcut in the config file

```
vim config
//add the following lines:

Host domainName
Hostname domain.net
IdentityFile ~/.ssh/domain_rsa
User www
```

_"User" is the username for logging over ssh_
