 `wrapserv` is a script for running "service action" commands (e.g. `docker restart`) with all the init specific commands/arguments abstracted away. Works with `systemd`, `upstart`, `sysv` inits.

You just need to put the service name and desired action. Examples:

```
wrapserv docker restart
wrapserv nginx reload
```

Multiple "service action" pairs are supported too e.g.

```
wrapserv docker stop nginx status postfix start
```
here, `wrapserv` will first stop `docker` (`dokcer stop`); if succeeds, will
check the status of `nginx` (`nginx status`), and if that succeeds, will start
`postfix` (`postfix start`), in that order. So, this is essentially same as
running
```
wrapserv docker stop && wrapserv nginx status && wrapserv postfix start
```
In `systemd`, you can specify the unit types too e.g.
```
wrapserv nginx.service reload
wrapserv timers.target status
```
If you just want to know the name of the init you have:

```
wrapserv -i
wrapserv --show-init-name
```
---

**How to install:**

1. Get the script:
 - Either `clone` the repository (**recommended**):
   ```git clone git@gitlab.com:heemayl/wrapserv.git```
    - Or download the script (in raw): ```wget https://gitlab.com/heemayl/wrapserv/raw/master/wrapserv```
    2. Make the script executable (if you're downloading only the script directly): ```chmox +x wrapserv```
    3. Put the script in any place in your `PATH`; best to have your `~/bin/` in `PATH` and put it there, or you can put it any standard directory e.g. `/usr/local/bin/` (if available) or `/usr/bin/`. Here, i'm putting the script in `/usr/bin/`: ```sudo mv wrapserv /usr/bin/```
    4. Creating a symlink would do too: ```sudo ln -s "$PWD"/wrapserv /usr/bin/```

**NOTE:** The advantage of using `ln` over direct `cp`/`mv` is that when you `pull` (`git pull ...` or `wget`/`curl` download) new changes from upstream, you don't need to `cp`/`mv` these every time. Another option would be to put the directory itself in the `PATH`, but this is not recommend.

---
