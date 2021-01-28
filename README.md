# dwmblocks

Modular status bar for dwm written in C.

## Patches

- [statuscmd](https://dwm.suckless.org/patches/statuscmd) and [statuscmd-signal](https://gist.github.com/danbyl/54f7c1d57fc6507242a95b71c3d8fdea#file-dwmblocks-statuscmd-signal-diff)

## Use

- Edit [blocks.h](./blocks.h) with the scripts you want to show in the status bar (consider adding them in `$PATH` for quick access), giving them an update interval and an update signal. For example:

```c
/* Icon  Command   Interval  Signal */
{  "",   "volume", 0,        11     },
{  "",   "clock",  1,        10     },
```

- In this case, the `volume` script has an update interval of `0` seconds. This avoids useless updates, but the block will need a manual update in order to display correct information in dwmblocks. You can manually update a block by using the update signal:

```sh
pkill -RTMIN+11 dwmblocks
```

- Add mouse support to the block by using the `BLOCK_BUTTON` variable inside the script:

```sh
#!/bin/sh

# volume script

case BLOCK_BUTTON in
  1) setsid -f "$TERMINAL" -e pulsemixer ;; # on left click, open pulsemixer
  3) pamixer -t ;;                          # on right click, toggle mute

# information to show in dwmblocks
[ $(pamixer --get-mute) = true ] && echo "MUTED" && exit
echo "VOL $(pamixer --get-volume)%"
```
