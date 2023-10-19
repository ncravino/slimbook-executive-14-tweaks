# Slimbook Executive 14 tweaks

A bunch of useful tweaks for this [laptop](https://slimbook.es/en/executive-en) tried under Debian Trixie. I have the version with the 11th gen i7.

## Keyboard

### Set a keycode (macro) for Fn (usually unset) via systemd service at start


- see/copy/modify `init-setkeycodes.service` to `/etc/systemd/system/` (see bellow for how to mod)
- `systemctl daemon-reload` to reload available services
- `systemctl enable init-setkeycodes.service` to enable it on every boot 
- `systemctl start init-setkeycodes.service` to start it (now)
- `systemctl status init-setkeycodes.service` to check the status

Note that this will only work in keyboards that map Fn to a scancode, usually you can see a warning in dmeg when you press Fn:
```
[   51.336674] atkbd serio0: Unknown key released (translated set 2, code 0xf8 on isa0060/serio0).
[   51.336687] atkbd serio0: Use 'setkeycodes e078 <keycode>' to make it known.
```

In this case the scancode for Fn is e078.

You can select an unused keycode to do this via `xmodmap -pke`, I used macro which is 120, for setkeycodes you need to remove 8 so it's 112.

### Use [keyd](https://github.com/rvaiya/keyd/) to set keycode for Fn (macro) to map Fn+delete to insert (which is missing in this laptop)

- clone the repository
- make and install keyd: `make install`
- see/copy/modify+copy the keyd.conf file to `/etc/conf` (see above+bellow+man for info and what to mod)
- enable keyd service if not enabled
- restart or reload keyd
- verify with `xev` if it's working

Keyd is akin to QMK but running as a virtual keyboard, and let's us work with layers. I just added one for Fn and added delete being mapped to insert.

```
[ids]

*

[main]

macro = layer(macro_layer)

[macro_layer]

delete = insert

```

## Power management 

### TLP 

- install tlp via apt
- verify if you're running the correct driver scaling driver (e.g. intel_pstate on modern intel)
- see/copy/modify+copy `tlp.conf` to `/etc/tlp/`

TLP allows us to control exactly how the laptop behaves when disconnected from power, and extend battery life.

See `tlp.conf` the configuration, but mostly it:
- reduces GPU and CPU clock
- changes power modes



