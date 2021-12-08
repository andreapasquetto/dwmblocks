# dwmblocks

A modular status bar for `dwm` written in C.

> All settings can be modified in [blocks.h](./blocks.h).

## Installation

1. Clone the repository:

    ```sh
    git clone https://github.com/andreapasquetto/dwmblocks
    ```

2. Make changes to the configuration

3. Compile and install:

    ```sh
    make && make clean install
    ```

## Use

Add all the scripts that the status bar must show by editing the `blocks` array:

- `icon` is for showing a custom icon of the script
- `command` is the name of the script
- `update interval` is how often the script will be triggered
- `update signal` is for a manual trigger of the script

```c
static const Block blocks[] = {
//  icon  command   update interval  update signal
  { "",   "volume", 0,               11            },
  { "",   "clock",  1,               10            },
};
```

In this example, the volume script has an update of `0` seconds. \
This avoids useless updates, but the block will need to be triggered manually in order to display correct information in the status bar. \
To manually trigger a block, use its update signal:

```sh
pkill -RTMIN+11 dwmblocks
```

> Consider adding the scripts to the `PATH` variable for a quick access.

### Mouse support

Mouse support can be added to the block by using the `BLOCK_BUTTON` variable inside the script:

```sh
#!/bin/sh
# volume script for dwmblocks

case $BLOCK_BUTTON in
  1) setsid -f "$TERMINAL" -e pulsemixer ;; # on left click, open pulsemixer
  2) pamixer -t ;;                          # on right click, toggle mute

# this is the information that the script will show
[ $(pamixer --get-mute) = true ] && echo "MUTED" && exit
echo "VOL $(pamixer --get-volume)%"

```

## Patches

- [statuscmd](https://dwm.suckless.org/patches/statuscmd) and [statuscmd-signal](https://gist.github.com/danbyl/54f7c1d57fc6507242a95b71c3d8fdea#file-dwmblocks-statuscmd-signal-diff)
