# Naming-Scheme

A revision of [mnx.io's](https://mnx.io/blog/a-proper-server-naming-scheme/) naming scheme and wordlist inspired from [tothink.com](http://web.archive.org/web/20091003023412/http://tothink.com/mnemonic/wordlist.txt)


# Overall

The naming scheme is inspired by the wisdom of the old elders, while inheriting some of the newer aspects of VMs, containers, and our newer nuke-pave world

## Physical vs Virtual Hosts

There are a few main types of general computing hosts in todays age, There are Bare Metal devices, VMs, Cloud Instances, and Containers. 

Bare Metal hosts are limited, long lived devices, they take physical space, require hands on work, and are cared for untill they are sold or scrapped.

VMs and cloud instances take no (physical) space, are created to do a spesific thing, they borrow space on other servers, but typically have their own IPs, Storage, and individual identites. 

Cloud Instances borrow no space, but exsist in spesific areas only. 

Containers are purely services, they take space and IPs on other infra.

### Physical Assets

Physical hosts should have uniq names, they have multiple uses throughout their life, Are able to be phyiscally viewed by other people, Physically moved, have multiple interfaces, etc.

`wordlist.txt` is a list of words chosen for their uniq phonetic ability to be easily repeated over the phone, unlike "Vhost1" or "ESXI12". A label on the front and rear of these servers would allow anyone with any lack of techincal sense to identify a device, and most important, do not allow for individuals to have numbers swapped in their head and interact with the wrong equipment. 

If you work with an entity that requires asset tags, they can also be used, they should be prepended with your intials "WT012345" but do not expect anyone nontechincal to be able to help you with your servers. A CMDB or spreadsheet is required for this level of naming, a Label that can be scanned into a public portal is best (snipe-it is pretty legit at this)

In terms of DNS. A records are perfered, but do plan to sacrafice a root domain to this type of naming, Ive found that mentaly working with your main domain that you want to use to expose to the world publicly being taken your are domain for everything can lead to fustraction and limitations, if your domain is "example.com" grab "exmpl.net" or another shorthand version of your domain.

Example

```bind
Taco.exmpl.net A 192.168.1.1
```


### Virtual Assets

VMs are generaly single use machines that either host a whole application or parts of it. they do not need a full name, but a shortname name for their use.

```
web01.exmpl.net
web02.exmpl.net
db01.exmpl.net
db02.exmpl.net
```

If you have a single one of that type of VM, then using `web` or `db` by itself is fine.

## Organizational names

If your platform exsits in more than one location, or better yet, you have more than one platform, then filling your l1.subdomains is simply not enough for simple discriptors like 'web01'.


### Location Identifiers

I oringally tried to standardize on naming each location after their street address, such as "datacenter 120" or "edge 8767", but that doesnt work well when interacting with others.

Thankfuully we do not have to make this standard, it is already created as by UN/LOCODE which piggybacks off of the 2 chara Country IDs, and then an additional 3 char for a city, since UN/LOCODE is based on the Transportation indestry of boat, rail, post offices and others, Many cities will be included. albeit with some weird 3 letters at some point.

Why not Airport codes? There are fewer airports, so their codes are much more generic. I was able to find at least 4 codes for UN/LOCODE in my area, while there are only 2 airport. 

An easier form to digest form of this is located at https://github.com/datasets/un-locode

#### Non important locations

If your are managing network devices, you might be working with devices at locations who's actual location actual does not matter, such as a end user, edge device, or GSM modem. its better to give these location at least 5 letter identifiers and use DDNS, like a CUSTID, zip code, or a vanity name. (mom.c.exmpl.net, 123456.c.exmpl.net) 


### Platform identifiers 

Most VMs and hosts today live inside of a platform, such as K8s or ESXI and do not live past it. Cloud providers can also be considered platforms, but their identifiers may work better as a country vs a platform due to their worldwide physical location nature, although I recommend using their 3 chara shorthands to prevent interaction with ISO 3166-1 (1.east.amz.exmpl.net vs 1.east.az.exmpl.net), assets within a platform will not likely interact with another platform directly. 

Platforms can either be considered *worldwide* and take a l1 subdomain, or can exsist under a location. like k8s.dio.exmpl.net

#### Cloud Providers


|      Provider       | Shorthand |
|:-------------------:|:---------:|
|    Digital Ocean    |    DIO    |
|       Linode        |    LIH    |
| Amazon Web Services |    AMZ    |
|    Google Cloud     |    GCE    |
|   Microsoft Azure   |    AZU    |
|        Vultr        |    VLR    |
|                     |           |

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

Switches can simply be named based on their location and an index. But routers may need globaly uniq names, we can use the `R` shortcode to put them in their own group

```
Examples

router1.ATX.exmpl.net A Public DDNS
gw.ATX.exmpl.net CNAME router1.atx
atx.r.exmpl.net CNAME gw.atx
```


