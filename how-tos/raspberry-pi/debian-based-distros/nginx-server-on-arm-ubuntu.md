# Nginx Server on ARM Ubuntu

{% hint style="info" %}
If it's marked with "atleast," that's the base package you need at least.. 

For "extra credit," you can install these other suggested packages to configure or play with on your own. I personally play with almost everything by nature, so you can't expect me to pass up a suggested great time.
{% endhint %}

```text
# atleast
sudo apt install nginx

# extra credit
sudo apt install libgd-tools fcgiwrap nginx-doc ssl-cert

# ssl-cert is a simple wrapper for OpenSSL's certificate request 
# utility that feeds it with the correct user variables. If you plan
# to use openssl, this is handy for creating unattending installs.
```

