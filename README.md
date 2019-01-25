acl localnet src 192.168.0.0/16acl localnet src 10.0.0.0/8

acl workintra src 172.30.100.0/23

acl localnet src fc00::/7acl localnet src fe80::/10

acl SSL_ports port 443

acl Safe_ports port 80acl Safe_ports port 21acl Safe_ports port 443acl Safe_ports port 70acl Safe_ports port 210acl Safe_ports port 1025-65535acl Safe_ports port 280acl Safe_ports port 488acl Safe_ports port 591acl Safe_ports port 777acl CONNECT method CONNECT

# Authentication

auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwords
auth_param basic realm proxy
acl authenticated proxy_auth REQUIRED
http_access allow authenticated

http_access allow manager localhost

http_access deny manager

http_access deny !Safe_ports

http_access deny CONNECT !SSL_ports

http_access allow localnet workintra

http_access allow localhost

# http_access allow all

http_port 3128

cache_mem 512 MB

maximum_object_size_in_memory 3 MB

memory_replacement_policy heap GDSF

cache_replacement_policy heap LFUDA

cache_dir ufs /var/spool/squid3 100 16 256

maximum_object_size 1 GB

cache_store_log /var/log/squid3/store.log

coredump_dir /var/spool/squid3

url_rewrite_program /etc/squid3/url_rewrite

acl url_rewrite_list dstdomain .google.com .google.de .tiles.virtualearth.net

acl url_rewrite_list dstdomain map.org .tile.cloudmade.com

acl url_rewrite_list dstdomain .tile.openstreetmap.org .tile.opencyclemap.org .tiles.mapbox.com .wheelmap.org .www.toolserver.org .tiles.osm2world.org .skobbler.net .tile.openstreetmap.de

acl url_rewrite_list dstdomain .itoworld.com .map.f4-group.com

url_rewrite_access allow url_rewrite_list

url_rewrite_access deny all

include /etc/squid3/refresh_pattern_specific.conf

include /etc/squid3/refresh_pattern.conf

quick_abort_min 100 KB

quick_abort_max 200 MB

quick_abort_pct 20

range_offset_limit 500 KB

via off

request_header_access From deny all

request_header_access Server deny all

cache_mgr ypid23@aol.de

check_hostnames on

dns_v4_first on

forwarded_for delete

refresh_all_ims on

reload_into_ims on


# Add monitoring stuff

# restricted url’s list such as some social networks, adult sites

# file must contain one domain per line

acl restricted_urls url_regex -i "/etc/squid/restricted_urls"

# anonymous proxies, web anonymizers and so on

# file must contain one domain per line (example: domain. )

acl anonymous_list url_regex  -i “/etc/squid/anon_proxy.list”

# working hours of Monitis’ office (just an example)

acl WorkingHours_Monitis time D 09:00-18:00

# working hours of Netangels’ office (just an example)

acl WorkingHours_Netangels time D 10:00-19:00

# Netangel’s local network range (just an example)

acl Netangels_Net src 192.168.10.0/24

# Monitis’ local network range (just an example)

acl Monitis_Net src 192.168.20.0/24

acl Sec_Perimeter src Netangels_Net Monitis_Net

# deny access for some resources in working hours

http_access allow WorkingHours_Monitis !restricted_urls

http_access allow WorkingHours_Netangels !restricted_urls

# deny access to anonymizers

http_access deny anonymous_list

# preventing cache hits for local resources

acl NetangelsLocal dstdomain .netangels.net

acl MonitisLocal dstdomain .monitis.com

# deny caching for nearby servers, because they don't benefit too

# much from cache hits and for saving storage

no_cache deny NetangelsLocal

no_cache deny MonitisLocal

#

# deny access for unwanted networks

http_access allow Sec_Perimeter
