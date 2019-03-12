# nmap

{% hint style="info" %}
If you're battling filtered ports, hit that firewall with a bunch of ACK packets.  Firewalls treat ACK packet as the response of the SYN packet.
{% endhint %}

```text
# Only send Ack packets to target
nmap -sA 192.168.1.6
```

```text
# http-enum script
nmap -sV -script=http-enum 192.168.2.7
Starting Nmap 7.70 ( https://nmap.org ) at 2019-03-12 00:36 PDT
Nmap scan report for 192.168.2.7
Host is up (0.00045s latency).
Not shown: 997 filtered ports
PORT    STATE  SERVICE  VERSION
22/tcp  closed ssh
80/tcp  open   http     Apache httpd
| http-enum: 
|   /admin/: Possible admin folder
|   /admin/index.html: Possible admin folder
|   /wp-login.php: Possible admin folder
|   /robots.txt: Robots file
|   /readme.html: Wordpress version: 2 
|   /feed/: Wordpress version: 4.3.1
|   /wp-includes/images/rss.png: Wordpress version 2.2 found.
|   /wp-includes/js/jquery/suggest.js: Wordpress version 2.5 found.
...

...
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 80.05 seconds

```



