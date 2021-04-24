# Naming-Scheme

A revision of [mnx.io's](https://mnx.io/blog/a-proper-server-naming-scheme/) naming scheme and wordlist inspired from [tothink.com](http://web.archive.org/web/20091003023412/http://tothink.com/mnemonic/wordlist.txt)


# Overall

The naming scheme is inspired by the wisdom of the old elders, while inheriting some of the newer aspects of VMs, containers, and our newer nuke-pave world

## Physical vs Virtual Hosts

There are a few main types of general computing hosts in today's age, There are Bare Metal devices, VMs, Cloud Instances, and Containers. 

Bare Metal hosts are limited, long lived devices, they take physical space, require hands on work, and are cared for until they are sold or scrapped.

VMs and cloud instances take no (physical) space, are created to do a specific thing, they borrow space on other servers, but typically have their own IPs, Storage, and individual identities. 

Cloud Instances borrow no space, but exist in specific areas only. 

Containers are purely services, they take space and IPs on other infra.

### Physical Assets

Physical hosts should have unique names, they have multiple uses throughout their life, Are able to be physically viewed by other people, Physically moved, have multiple interfaces, etc.

`wordlist.txt` is a list of words chosen for their unique phonetic ability to be easily repeated over the phone, unlike "Vhost1" or "ESXI12". A label on the front and rear of these servers would allow anyone with any lack of technical sense to identify a device, and most important, do not allow for individuals to have numbers swapped in their head and interact with the wrong equipment. 

If you work with an entity that requires asset tags, they can also be used, they should be prepend with your initials "WT012345" but do not expect anyone nontechnical to be able to help you with your servers. A CMDB or spreadsheet is required for this level of naming, a Label that can be scanned into a public portal is best (snipe-it is pretty legit at this)

In terms of DNS. A records are preferred, but do plan to sacrifice a root domain to this type of naming, I've found that mentally working with your main domain that you want to use to expose to the world publicly being taken your are domain for everything can lead to frustration and limitations, if your domain is "example.com" grab "exmpl.net" or another shorthand version of your domain.

Example

```bind
Taco.exmpl.net A 192.168.1.1
```


### Virtual Assets

VMs are generally single use machines that either host a whole application or parts of it. they do not need a full name, but a short-name name for their use.

```
web01.exmpl.net
web02.exmpl.net
db01.exmpl.net
db02.exmpl.net
```

If you have a single one of that type of VM, then using `web` or `db` by itself is fine.

## Organizational names

If your platform exists in more than one location, or better yet, you have more than one platform, then filling your l1.subdomains is simply not enough for simple descriptors like 'web01'.


### Location Identifiers

I originally tried to standardize on naming each location after their street address, such as "datacenter 120" or "edge 8767", but that doesn't work well when interacting with others.

Thankfully we do not have to make this standard, it is already created as by UN/LOCODE which piggybacks off of the 2 character Country IDs, and then an additional 3 char for a city, since UN/LOCODE is based on the Transportation industry of boat, rail, post offices and others, Many cities will be included. albeit with some weird 3 letters at some point.

Why not Airport codes? There are fewer airports, so their codes are much more generic. I was able to find at least 4 codes for UN/LOCODE in my area, while there are only 2 airport. 

An easier form to digest form of this is located at https://github.com/datasets/un-locode

#### Non important locations

If your are managing network devices, you might be working with devices at locations who's actual location actual does not matter, such as a end user, edge device, or GSM modem. its better to give these location at least 5 letter identifiers and use DDNS, like a CUSTID, zip code, or a vanity name. (mom.c.exmpl.net, 123456.c.exmpl.net) 


### Platform identifiers 

Most VMs and hosts today live inside of a platform, such as K8s or ESXI and do not live past it. Cloud providers can also be considered platforms, but their identifiers may work better as a country vs a platform due to their worldwide physical location nature, although I recommend using their 3 character shorthand to prevent interaction with ISO 3166-1 (1.east.amz.exmpl.net vs 1.east.az.exmpl.net), assets within a platform will not likely interact with another platform directly. 

Platforms can either be considered *worldwide* and take a l1 subdomain, or can exist under a location. like k8s.dio.exmpl.net

#### Cloud Providers


|      Provider       | Shorthand |
|:-------------------:|:---------:|
|    Digital Ocean    |    DIO    |
|       Linode        |    LIN    |
| Amazon Web Services |    AMS    |
|    Google Cloud     |    GCP    |
|   Microsoft Azure   |    AZU    |
|        Vultr        |    VLR    |

### Single char identifiers 

A few examples if you decide you want another organizational group


| Shorthand | Description |
|:---------:|:-----------:|
|     u     |    User     |
|     h     |    Host     |
|     g     |   Gateway   |
|     r     |   Router    |
|     c     |  Customer   |
|           |             |

### Networking Equipment

Most networking equipment is based on a location and has a single use. They could have a Host-esk name, but its not required. 

Switches can simply be named based on their location and an index. But routers may need globally unique names, we can use the `R` shortcode to put them in their own group

```
Examples

router1.ATX.exmpl.net A Public DDNS
gw.ATX.exmpl.net CNAME router1.atx
atx.r.exmpl.net CNAME gw.atx
```

## Putting it all together


Lets put together a simple Homelab and apply the naming.

![Simple Diagram](https://viewer.diagrams.net/?highlight=0000ff&edit=_blank&layers=1&nav=1#R7VpLc9owEP41HMPYll85JoS2B9rJNIe2p45iC1uN7GVkGUx%2FfSUjv3AeZMYEmJoL6NvVa%2Ff7VrKHCZolxWeOV%2FFXCAmbWEZYTNDdxLJMy3Tll0K2GjENjUSchhprgAf6l2jQ0GhOQ5J1HAUAE3TVBQNIUxKIDoY5h03XbQmsO%2BsKR6QHPASY9dEfNBTxDvUdo8G%2FEBrFot6ftiS4ctZAFuMQNi0IzSdoxgHE7ldSzAhT0avisuv36QVrvTBOUnFIhyLy%2FJsFQlf%2B%2FXLp%2Bdm3m%2B%2FWVRXmTGyrHZNQBkA3gYsYIkgxmzfoLYc8DYka1pCtxmcBsJKgKcE%2FRIitzibOBUgoFgnT1t2caqIX96KhDHIekNc2oDmBeUTEK36ojrjkKoGECL6V%2FThhWNB1dx1Ycyaq%2FZqwyh86su%2BJ8m7cNWa5nqkX9VZ0IBeMpmRWM1pFeUkZmwEDXrqjWfmReCY4PJGWxfV98xZJS8RxSGU8K1sKKWnBd5TL0SmkpYkrsurBKp5bConxSq0wKSIl8GlKxAb4UzYNGOShWhc0M8jZl%2BWnTvGacEGK15PcT4rugHxNzqp2VKLbNEK81lDc0mDlNnwazV7aPkAsMlp8%2B1P1nzpV85cermzcFZ3Wtt26J5zKzROuwQGVhw5Unn1K5aG3lbfHYMNw3VJZirtUHgQ3jEZKJELlq0YX%2BJGwe8ioltAjCAGJdGB7hkAGVoX%2FFutxauDSJJ%2FI0z0ZSNqe05G25falXZ%2BkbW2jo2m7T4z%2FWNv2gdp2Tqlte9T2YNqWTN4t%2FAjiRqcXN7rwW65zoB79k95y7bGEvj9l3ilT5owldLASmm2oCOLjlFD75CXUG5kyHFMIXw912Dqo%2B5Ds2Ic9JNfsGZwp%2FsiU4ZgigJcvKIegimW%2FTZVni8rxuGKOB9BwZFlTLnLMfg9aXix7anod2nh%2Bnza2M%2FVRnzjHexPnjry5ON5YZ8Cb8Rpz5rzZe%2Bd%2FFqQZbzRnTpp%2BsXGfeWD6cN5cj7w5b97sFZtzIE1V70bSXAhpjnsdls3mHyKlrfVHGzT%2FBw%3D%3D)

First, we define a FQDN to use as our network domain, We will use example.com. If you want to use your existing domain, use infra.example.com

Lets name our general use devices

We will name our san brenda, and our compute host parker. 

```bind
brenda IN A 192.168.1.10
parker IN A 192.168.1.20
```

Our router is also our gateway, so we can get away with calling it the gateway of our network, or give it a generic name. 

To prove how flexible UN/LOCODE is. I will pick a random small town. Crowell TX, US-5CM

```bind
gateway.5CM.example.com in A [Updated via DDNS]
5CM.example.com in CNAME gateway.5CM
```

Now lets give our servers function names, 

```
nas.5CM IN CNAME brenda
ESXI01.5CM IN CNAME parker
```

> But rwaltr, your dns names are so verbose!

Use search domains, Set your DHCP servers to search domains from the most specific to the most generic based on their location

### Multiple Sites

Lets add a new site, S2K


```
gateway.s2k IN A [DDNS]
s2k IN CNAME gateway.s2k
```


We want to be able to make routes between these two locations, possibly over IPsec, Wireguard, or GRE, but the problem we will face is that these routers will have 2 IPs, their local IP (or, more preferably, their loopback interface) and their public IP.

We can use our r.example.net to create router loopback entries, while using their public names for public IPs.

```
#S2K
gateway.s2k IN A [DDNS]
s2k IN CNAME gateway.s2k
s2k.r IN A 10.10.1.20

#5CM
gateway.5cm in A [DDNS]
5cm in CNAME gateway.5CM
5cm.r in A 10.10.1.30
```

### Edge Locations

Your dear sweet mother lets you managed the router. But lets be honest, Her phyiscal location doesnt matter. 

```bind
gateway.mom IN A [DDNS]
mom IN CNAME gateway.mom
#OR
gateway.mom.c IN A [DDNS]
mom.c IN CNAME gateway.mom.c
```
