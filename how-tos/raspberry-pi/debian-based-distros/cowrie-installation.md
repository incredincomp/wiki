# Cowrie Installation

[(BORROWED FROM https://cowrie.readthedocs.io/en/latest/INSTALL.html)]

Installing Cowrie in seven steps.

## Step 1: Install dependencies
First we install system-wide support for Python virtual environments and other dependencies. Actual Python packages are installed later.

On Debian based systems (last verified on Debian 9, 2017-07-25): For a Python3 based environment:

```text
$ sudo apt-get install git python-virtualenv libssl-dev libffi-dev build-essential libpython3-dev python3-minimal authbind

# Or for Python2:

$ sudo apt-get install git python-virtualenv libssl-dev libffi-dev build-essential libpython-dev python2.7-minimal authbind
```

## Step 2: Create a user account
It’s strongly recommended to run with a dedicated non-root user id:
```text
$ sudo adduser --disabled-password cowrie
Adding user 'cowrie' ...
Adding new group 'cowrie' (1002) ...
Adding new user 'cowrie' (1002) with group 'cowrie' ...
Changing the user information for cowrie
Enter the new value, or press ENTER for the default
Full Name []:
Room Number []:
Work Phone []:
Home Phone []:
Other []:
Is the information correct? [Y/n]

$ sudo su - cowrie
```

## Step 3: Checkout the code
Check out the code:
```text
$ git clone http://github.com/cowrie/cowrie
Cloning into 'cowrie'...
remote: Counting objects: 2965, done.
remote: Compressing objects: 100% (1025/1025), done.
remote: Total 2965 (delta 1908), reused 2962 (delta 1905), pack-reused 0
Receiving objects: 100% (2965/2965), 3.41 MiB | 2.57 MiB/s, done.
Resolving deltas: 100% (1908/1908), done.
Checking connectivity... done.

$ cd cowrie
```
## Step 4: Setup Virtual Environment
Next you need to create your virtual environment:

```text
$ pwd
/home/cowrie/cowrie
$ virtualenv --python=python3 cowrie-env
New python executable in ./cowrie/cowrie-env/bin/python
Installing setuptools, pip, wheel...done.
```
Alternatively, create a Python2 virtual environment:
```text
$ virtualenv --python=python2 cowrie-env
New python executable in ./cowrie/cowrie-env/bin/python
Installing setuptools, pip, wheel...done.
```

Activate the virtual environment and install packages:
```text
$ source cowrie-env/bin/activate
(cowrie-env) $ pip install --upgrade pip
(cowrie-env) $ pip install --upgrade -r requirements.txt
```
## Step 5: Install configuration file
The configuration for Cowrie is stored in cowrie.cfg.dist and cowrie.cfg. Both files are read on startup, where entries from cowrie.cfg take precedence. The .dist file can be overwritten by upgrades, cowrie.cfg will not be touched. To run with a standard configuration, there is no need to change anything. To enable telnet, for example, create cowrie.cfg and input only the following:
```text
[telnet]
enabled = true
```

## Step 6: Starting Cowrie
Start Cowrie with the cowrie command. You can add the cowrie/bin directory to your path if desired. An existing virtual environment is preserved if activated, otherwise Cowrie will attempt to load the environment called “cowrie-env”:
```text
$ bin/cowrie start
Activating virtualenv "cowrie-env"
Starting cowrie with extra arguments [] ...
```

## Step 7: Listening on port 22 (OPTIONAL)
There are three methods to make Cowrie accessible on the default SSH port (22): iptables, authbind and setcap.

## Iptables
Port redirection commands are system-wide and need to be executed as root. A firewall redirect can make your existing SSH server unreachable, remember to move the existing server to a different port number first.

The following firewall rule will forward incoming traffic on port 22 to port 2222 on Linux:
```text
$ sudo iptables -t nat -A PREROUTING -p tcp --dport 22 -j REDIRECT --to-port 2222
```

Or for telnet:
```text
$ sudo iptables -t nat -A PREROUTING -p tcp --dport 23 -j REDIRECT --to-port 2223
Note that you should test this rule only from another host; it doesn’t apply to loopback connections.
```
On MacOS run:
```text
$ echo "rdr pass inet proto tcp from any to any port 22 -> 127.0.0.1 port 2222" | sudo pfctl -ef -
```

## Authbind
Alternatively you can run authbind to listen as non-root on port 22 directly:
```text
$ sudo apt-get install authbind
$ sudo touch /etc/authbind/byport/22
$ sudo chown cowrie:cowrie /etc/authbind/byport/22
$ sudo chmod 770 /etc/authbind/byport/22
```

Edit bin/cowrie and modify the AUTHBIND_ENABLED setting

Change the listening port to 22 in cowrie.cfg:
```text
[ssh]
listen_endpoints = tcp:22:interface=0.0.0.0
```
Or for telnet:
```text
$ apt-get install authbind
$ sudo touch /etc/authbind/byport/23
$ sudo chown cowrie:cowrie /etc/authbind/byport/23
$ sudo chmod 770 /etc/authbind/byport/23
```
Change the listening port to 23 in cowrie.cfg:
```text
[telnet]
listen_endpoints = tcp:2223:interface=0.0.0.0
```

Setcap
Or use setcap to give permissions to Python to listen on ports<1024:
```text
$ setcap cap_net_bind_service=+ep /usr/bin/python2.7
```

And change the listening ports in cowrie.cfg as above.

##Running using Supervisord (OPTIONAL)
On Debian, put the below in /etc/supervisor/conf.d/cowrie.conf:
```text
[program:cowrie]
command=/home/cowrie/cowrie/bin/cowrie start
directory=/home/cowrie/cowrie/
user=cowrie
autorestart=true
redirect_stderr=true
```
Update the bin/cowrie script, change:
```text
DAEMONIZE=""
to:

DAEMONIZE="-n"
```

## Configure Additional Output Plugins (OPTIONAL)
Cowrie automatically outputs event data to text and JSON log files in var/log/cowrie. Additional output plugins can be configured to record the data other ways. Supported output plugins include:

Cuckoo
ELK (Elastic) Stack
Graylog
Kippo-Graph
Splunk
SQL (MySQL, SQLite3, RethinkDB)
See ~/cowrie/docs/[Output Plugin]/README.rst for details.

## Troubleshooting
If you see twistd: Unknown command: cowrie there are two
possibilities. If there’s a Python stack trace, it probably means there’s a missing or broken dependency. If there’s no stack trace, double check that your PYTHONPATH is set to the source code directory.
Default file permissions

To make Cowrie logfiles public readable, change the --umask 0077 option in start.sh into --umask 0022

## Updating Cowrie
Updating is an easy process. First stop your honeypot. Then fetch updates from GitHub, and upgrade your Python dependencies:
```text
bin/cowrie stop
git pull
pip install --upgrade -r requirements.txt
```
If you use output plugins like SQL, Splunk, or ELK, remember to also upgrade your dependencies for these too:
```text
pip install --upgrade -r requirements-output.txt
```

And finally, start Cowrie back up after finishing all updates:
```text
bin/cowrie start
```

## Modifying Cowrie
The pre-login banner can be set by creating the file honeyfs/etc/issue.net. The post-login banner can be customized by editing honeyfs/etc/motd.
