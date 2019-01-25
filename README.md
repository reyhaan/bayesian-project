acl localnet src 192.168.0.0/16acl localnet src 10.0.0.0/8

acl workintra src 172.30.100.0/23

acl localnet src fc00::/7acl localnet src fe80::/10

acl SSL_ports port 443

acl Safe_ports port 80acl Safe_ports port 21acl Safe_ports port 443acl Safe_ports port 70acl Safe_ports port 210acl Safe_ports port 1025-65535acl Safe_ports port 280acl Safe_ports port 488acl Safe_ports port 591acl Safe_ports port 777acl CONNECT method CONNECT

http_access allow manager localhost

http_access deny manager

http_access deny !Safe_ports

http_access deny CONNECT !SSL_ports

http_access allow localnet workintra

http_access allow localhost

http_access deny all

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
