http_port 3128 transparent
dns_v4_first on
cache deny all
forwarded_for delete
tcp_outgoing_address 10.111.1.149
via off
auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwords
auth_param basic realm proxy
acl authenticated proxy_auth REQUIRED
http_access allow authenticated
http_access deny all
