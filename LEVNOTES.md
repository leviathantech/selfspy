### console log on install (`$ sudo make install`):

```sh
mkdir -p /var/lib/selfspy
install selfspy/*.py /var/lib/selfspy
ln -s /var/lib/selfspy/__init__.py /usr/bin/selfspy
ln -s /var/lib/selfspy/stats.py /usr/bin/selfstats
```

Also run:

`$ sudo python setup.py install`

`$ sudo sqlite3 /Library/Application\ Support/com.apple.TCC/TCC.db "INSERT OR REPLACE INTO access values ('kTCCServiceAccessibility','/usr/local/bin/python',0,1,0,NULL);"`

* `$ virtualenv venv`
* `$ source venv/bin/activate`
* `$ pip install osx-requirements.txt`
* get tccutil.py from https://github.com/jacobsalmela/tccutil and copy into
  `/usr/sbin/`

In the end I followed https://github.com/benzguo/qself/blob/master/selfspy.md, running:
```sh
git clone git://github.com/gurgeh/selfspy
cd selfspy
sudo easy_install -U pyobjc-core
sudo python setup.py install
curl https://raw.github.com/benzguo/qself/master/com.github.selfspy.plist > ~/Library/LaunchAgents/com.github.selfspy.plist
```

By default python should be in `/usr/bin/python`, the selfspy executable is in `/usr/local/bin/selfspy` and selfspy data, etc.
is in `~/.selfspy/`.  If this is not the case, the plist file will have to be changed.

In order to get selfspy running on login you have to follow these instructions:

> To start the daemon, log out and log back in. Go to 
System Preferences > Security & Privacy > Privacy > Accessibility.
You should see python – give it accessibility access. You'll have to 
log out and log in again for selfspy to start tracking keypresses.

> Run selfstats --pactive, type some stuff, and run selfstats --pactive 
again. If everything is gravy, you'll see your keystroke count increase

This did not actually work for me as specified.  Instead I had to copy the `.plist` file by hand.  I also had to log in and out several times before Python showed up in the accessibility settings.  

I tried at one point to insert python and selfspy into accessibility myself using tccutil.py but that didn't actually seem to work correctly:

`sudo tccutil.py -i /usr/bin/python`
`sudo tccutil.py -i /usr/local/bin/selfspy`

I also had to grep for selfspy several times and remove selfspy-created locks several times:

`ps -ef | grep selfspy`
`rm ~/.selfspy/selfspy.pid.lock`

Obviously these issues need to be worked out to get this thing automated and usable for Telepath, since none of this should ever be seen by the user.
