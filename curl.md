# Networking
[<TIL](Programming.md)

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
