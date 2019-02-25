# Installing a Personal Web Server on a Pi

## Couple prerequisites before we even can start on the config:

* Raspberry Pi 3+
* SD or USB with Raspbian installed \(if you want to use it headless, which I recommend for ease, you need to activate SSH when you first boot it up or just follow this some way [https://www.raspberrypi.org/documentation/remote-access/ssh/](https://www.raspberrypi.org/documentation/remote-access/ssh/) if you want to just stay here, '''do;''' You’ll need to be hooked to a monitor with a keyboard at least for the Stretch version, then get to the raspi-config menu using `sudo raspi-config` and find SSH in the Interfaces tab, then activate it! Then you can log into your pi from a computer on your local network using SSH from a terminal of some sort. Hook that headless pi directly to your router, or hey, maybe a separate spare router that’s linked to your main router.. Networking is cool! Don’t limit yourself to your imagination
* A little determination and a lot of perseverance ;\)

### 1.\) Domain name

Got to go buy yourself a sweet domain name!!

{% hint style="info" %}
 There are lots of places to buy domain names from, a few examples would be [domains.google.com](https://domains.google.com), [GoDaddy.com](https://www.godaddy.com), and [NameCheap.com](https://www.namecheap.com)
{% endhint %}

### 2.\) DNS settings

This one goes hand in hand with the router config and talking to your ISP.. your dns settings are going to require you to put your public facing IP\(That’s the one your ISP gives your modem when it connects to their network,\) as the redirecting route for your domain name. This means that when someone types in www.yoursicksite.com, they will be redirected to your modem, which will then route the traffic to your router, which will then route the traffic directly to your webserver, which will then serve the website that we are about to build and host!

The security on this is debatable, as I haven’t audited a system running behind a private router before. I will probably be doing much more of that in the future and I will probably do a write up about interesting finds, so keep posted! I will attach links as they are generated…

{% hint style="info" %}
Have done some testing on my server since I first wrote this.  It is pretty well contained and I have yet to have any issues.
{% endhint %}

{% hint style="danger" %}
#### IF YOU LEAVE PORT 22 OPEN TO THE OUTSIDE WORLD FOR PERSONAL SSH ACCESS, YOUR SERVER \*WILL\* HAVE A LARGE AMOUNT OF ATTEMPTS TO INVALIDATE THE SECURITY AND INTEGRITY OF YOUR SERVER SYSTEM.  
{% endhint %}

{% hint style="info" %}
I would at least recommend changing your root/pi account name/password to something more difficult for people to brute force. There are many other ways to protect your outward facing servers from unauthorized access, but a complicated password and none default username will go extremely far in your successful hardening of your server.
{% endhint %}

### 3.\) Router Config + ISP info

I can't speak much to router config because that is going to be different depending on the manufacture of said router. Basically, what you need to do is isolate the IP of your pi server that is directly connected to your router, find it somehow, you’re probably smart\(you made it here didn’t you!?,\) you can do it. Once you find it, keep track of it and either change it to something that you can remember, \(that is either subnetted away from your normal devices\) or one inside your own DHCP network. The difference to this are debatable because of the security, but I’m naive so maybe this is a non-issue. As I learn more I will update my published information.

Anyway, you're going to have to have a kind of weird conversation with your ISP for this to work out for you properly. I have comcast, so I was certain that all of the work I had done up to this point would be for naught.

The conversation with your ISP is going to be something along the lines of, “I need to make my modem reachable from the internet. I am trying to set up a personal website for myself, it's going be like a personal documentation for my tech stuff, and I need to be able to access it when I am not at my house.” This gave the tech I was speaking with enough information about what I wanted to do, that they were able to change whatever settings on their end which then allowed you to be viewing this website right this very second! Crazy right?

Some ISP’s may not be so savvy to doing this to you if you don’t have a business account or something like that, as you could very well be opening their network up to various unsavory things in their opinion, like maybe one of those nasty, illegal file sharing websites…… Any who, your ISP isn’t going to appreciate heavy bandwidth usage, malware, or like, pretty much anything that you should damn well know better about anyway. We’ve already established that you’re smart, so you should already know not to be an unethical JA to the public in public. Things like these are a privilege, and if you need a refresher, check out [Kim Dotcom’s Wikipedia](https://en.wikipedia.org/wiki/Kim_Dotcom) page.

After this conversation, if everything goes well at least, you should be able to access your pi \(if you set up SSH when you built it up\) through SSH from anywhere \(security risks be heeded\) and that makes dealing with configuration much easier. Also, now that your IP is open to incoming traffic, you can let your router do the work of firewall and network route translation. If you set everything up on your router properly, opened up ports 80 \(and 443 if you are using SSL,\) and set up some sort of configuration so that everything plays nicely… you should now be able to start trying to do your ngnix install if you haven’t already, and then the dreaded nginx configuration\(it’s seriously not so bad.. just keep your file syntax clean and ordered properly.\)

### 4.\) Nginx install and config files

Alright, so installing nginx is relatively simple if you are directly mimicking me. Nginx 1.10.3 is working beautifully on Debian 9 stretch, kernel 4.14.71-v7+. So you should literally be able to

```bash
sudo apt-get install nginx
```

Okay, so this was my nginx config file that I had working the last time it was the thing serving my website\(now thank apache2. I still use nginx all the time for all sorts of stuff because of ease of use mind you. Most specifically, I use nginx as a reverse https proxy and .ht authentication on a local access ELK stack that I built for a local business\) I've tried to add comments in to explain what the heck I was doing but I can't say that I even know why it worked really. This was googling and googling and then doing it some more. **May the force be with you**

```bash
#Check nginx version before you start
$ sudo nginx -v
$ nginx version: nginx/1.10.3
```

So my old how to isnt really relevant with new nginx as the sites-available and sites-enabled directories are no longer included in the install. I just make them myself now because I am so used to it but I am not sure that is needed. That being said, without them, I do not know how nginx understands what it is supposed to do. Meh.

#### This is how I had my nginx.conf file set up before the great collapse

```bash
#/* listen is saying what port to listen on, 
#if this had ssl,youd put 443 here. */
#/* next part, default_server, is very very important. 
#This needs to be placed on at the least, one server 
#location config file.  I also have listen [::]:80; in 
#there so that no matter what gets requested, this server 
#is going to be what responds. ( I think...) */
server {
  listen 80 default_server;
  listen [::]:80;
#/* So, this here root line was one that initially confused 
#me, now I find it silly.  So, literally this just needs to be 
#a directory that is set to somewhere near 755 
#(security professionals maybe would argue. I dont know. 
#Make sure the name of this directory matches what the url 
#address will look like.  Like here, my dns settings had 
#www.incredincomp.com reroute to a subdomain called 
#wiki.incredincomp.com so that is what I named my directory */  
  root /var/www/wiki.incredincomp.com/;
#I couldnt rememeber why I was using this but its because I 
#was trying to get a php web app to work. It did for awhile 
#but things just seemed to not work out great on the pi. The 
#hardware is weird you know.
  location ~ [^/]\.php(/|$) {
    fastcgi_split_path_info ^(.+?\.php)(/.*)$;
    if (!-f $document_root$fastcgi_script_name) {
        return 404;
    }
    # Mitigate https://httpoxy.org/ vulnerabilities
    fastcgi_param HTTP_PROXY "";
    fastcgi_pass 127.0.0.1:9000;
    fastcgi_index index.php;
    # include the fastcgi_param setting
    include fastcgi_params;
  }
  
  location / {
          try_files $uri $uri/ /index.html =404;
  }
}
```

{% hint style="danger" %}
Any time you make a change to this file, you need to run:

`sudo restart service ngnix`

so that the server will restart and use the new configurations you've just set.
{% endhint %}

### 5.\) Common mistakes and things to look for

Okay, now that I broke something again, I can write something here! So, in my course of making this website some form of wiki page dealio, I was attempting to install MediaWiki on my pi, which is going better now mind you.. but initially, while installing the whole LAMP set up, \(apache, php, mariaDB, \) it somehow made my ngnix no longer load.. then I swear I don’t know how I did it but I changed a few settings according to the media wiki nginx page and now for some reason, apache is now seamlessly serving my webpage. This is fascinating and I don’t know what is going on. I will continue to investigate and post back with fixes, but my current recommendation would be “if you don’t know what you are doing, just stick with NGINX and some html5 and CSS. Im sure you could code a decent enough ‘wiki’ styled html page with some CSS help.”

TLDR: don’t install lamp after you set up nginx or you will have a head ache and be scared when your site is no longer reachable through nginx, with zero config setup on apache.

### 6.\) Helpful things to remember

sudo everything, seriously. Linux 101 is not to just log in as root all the time. That’s technically stupid, so don’t do it. Just type sudo and use your epically drafted password. **Don’t be silly,** _**hash your willy!**_

