# Blocking Pornography Sites and Turning on Google, Youtube, Bing Safe Search!

### Mikrotik
Blocking pornography sites and porn search results can be done easily via DNS but VPN is your enemy.
Make sure that if clients change their DNS settings redirect all DNS traffic to OpenDNS IPs (208.67.222.123 and 208.67.220.123).
```
ip firewall nat
add chain=dstnat in-interface-list=LAN port=53 protocol=udp action=dst-nat to-addresses=208.67.222.123
add chain=dstnat in-interface-list=LAN port=53 protocol=udp action=dst-nat to-addresses=208.67.220.123
```
If you are using Mikrotik you can add static entries using the following commnads.
```
/ip dns static
add address=216.239.38.120 name=restrictmoderate.youtube.com ttl=300 comment=ForceSafeSearch
add address=216.239.38.120 name=youtube.com ttl=300 comment=ForceSafeSearch
add address=216.239.38.120 name=www.youtube.com ttl=300 comment=ForceSafeSearch
add address=216.239.38.120 name=m.youtube.com ttl=300 comment=ForceSafeSearch
add address=216.239.38.120 name=youtubei.googleapis.com ttl=300 comment=ForceSafeSearch
add address=216.239.38.120 name=youtube.googleapis.com ttl=300 comment=ForceSafeSearch
add address=216.239.38.120 name=youtube-nocookie.com ttl=300 comment=ForceSafeSearch
add address=216.239.38.120 name=www.youtube-nocookie.com ttl=300 comment=ForceSafeSearch
add address=216.239.38.120 name=restrict.youtube.com ttl=300 comment=ForceSafeSearch
add address=216.239.38.120 name=forcesafesearch.google.com ttl=300 comment=ForceSafeSearch
add address=216.239.38.120 name=www.google.com.af ttl=300 comment=ForceSafeSearch
add address=216.239.38.120 name=google.com ttl=300 comment=ForceSafeSearch
add address=216.239.38.120 name=google.com.af ttl=300 comment=ForceSafeSearch
add address=216.239.38.120 name=www.google.com ttl=300 comment=ForceSafeSearch
add address=216.239.38.120 name=m.google.com ttl=300 comment=ForceSafeSearch
add address=216.239.38.120 name=m.google.com.af ttl=300 comment=ForceSafeSearch
add address=204.79.197.220 name=strict.bing.com ttl=300 comment=ForceSafeSearch
add address=204.79.197.220 name=bing.com ttl=300 comment=ForceSafeSearch
add address=204.79.197.220 name=www.bing.com ttl=300 comment=ForceSafeSearch
add address=204.79.197.220 name=m.bing.com ttl=300 comment=ForceSafeSearch
/ip dns cache flush
```
You can also enable safe search by adding the following DNS static entries to your local DNS server to force clients to use safe search enabled apps.
```
216.239.38.120 restrictmoderate.youtube.com
216.239.38.120 youtube.com
216.239.38.120 www.youtube.com
216.239.38.120 m.youtube.com
216.239.38.120 youtubei.googleapis.com
216.239.38.120 youtube.googleapis.com
216.239.38.120 youtube-nocookie.com
216.239.38.120 www.youtube-nocookie.com
216.239.38.120 restrict.youtube.com
216.239.38.120 forcesafesearch.google.com
216.239.38.120 google.com
216.239.38.120 www.google.com
216.239.38.120 m.google.com
216.239.38.120 google.com.af
216.239.38.120 www.google.com.af
204.79.197.220 strict.bing.com
204.79.197.220 bing.com
204.79.197.220 www.bing.com
204.79.197.220 m.bing.com
```
Thanks
