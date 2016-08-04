The team in West Africa is having some problems - they’re telling me that all their laptops (Lenovo 80LS) are on Windows 10 and they can’t connect to the POSM.

it just says “connecting” forever but won’t actually connect

we saw this in ecuador but were never able to figure out why it was happening (or even to reproduce it). i was hopeful that the changes to the wireless config had fixed it.

the workaround is to make the wifi network public

the current version of POSM Admin has a toggle to do this using the web interface.

[picture]

However, if you're running an older version of POSM, you'll need to make that change manually.

someone needs to log into the POSM (using `ssh root@posm.io` (password: `posm`) or by plugging a monitor and keyboard in and logging in as `root` (password: `posm`)) (edited)

and then edit `/etc/hostapd/hostapd.conf` (probably with `nano`)

there’s a line that looks like:
```
wpa=2
```

(it’s probably line 988)

it should be changed to `wpa=0`

(and then the file should be saved)

once that’s done, the POSM should either be restarted or `killall -HUP hostapd` can be run from the command line

(and that’s it, the `POSM` network will become public )
