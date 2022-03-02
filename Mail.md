# Mail

## SPF tools
If SPF record exceeds the 10 DNS query limit and you want to find out all of the senders do the
following DNS entry:
1. `maripusc.ro.  IN TXT "v=spf1 mx a ~all"` (where _a_ => All the A records for domain are tested.
   and _mx_ => All the A records for all the MX records for domain are tested.)
2. After that go to https://dmarcly.com/tools/spf-record-checker.
3. Go down to "Flatten SPF record" and replace the above DNS record with the one suggested there.
[src 1](https://dmarcly.com/tools/spf-record-checker)
[src 2](https://www.spfwizard.net/)

# SPF modifiers
| Mechanism | Explanation |
---
| all     | This mechanism always matches. It usually goes at the end of the SPF record.                                                                                                                                                                                                                    |
| include | The specified domain is searched for a match. If the lookup does not return a match or an error, processing proceeds to the next directive.                                                                                                                                                     |
| ip4     | The argument to the "ip4:" mechanism is an IPv4 network range. If no prefix-length is given, /32 is assumed (singling out an individual host address).                                                                                                                                          |
| ip6     | The argument to the "ip6:" mechanism is an IPv6 network range. If no prefix-length is given, /128 is assumed (singling out an individual host address).                                                                                                                                         |
| a       | All the A records for domain are tested. If the client IP is found among them, this mechanism matches. If the connection is made over IPv6, then an AAAA lookup is performed instead.                                                                                                           |
| mx      | All the A records for all the MX records for domain are tested in order of MX priority. If the client IP is found among them, this mechanism matches.                                                                                                                                           |
| ptr     | The hostname or hostnames for the client IP are looked up using PTR queries. The hostnames are then validated: at least one of the A records for a PTR hostname must match the original client IP. Invalid hostnames are discarded. If a valid hostname ends in domain, this mechanism matches. |
| exists  | Perform an A query on the provided domain. If a result is found, this constitutes a match.                                                                                                                                                                                                      |
|redirect 	         |For a modifier redirect=domain, the SPF record for domain replaces the current record.                                                                                                                                                                                                                                                                                                  |

[src](https://dmarcly.com/tools/spf-record-checker)


