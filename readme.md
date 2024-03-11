# zmk-config
My personal [ZMK firmware](https://github.com/zmkfirmware/zmk/) config for the [Swoop MX](https://github.com/jimmerricks/swoop) using a noname nice!nano v2 (nRF52840 MCU). Layout is based on [Miryoku](https://github.com/manna-harbour/miryoku_zmk), which is likely the best place to start, but for some reason I've had trouble . Thanks to [snicklepickles](https://github.com/snicklepickles/zmk-config) (who's repo I originally forked), and everyone else in the community who I've taken snippets and inspiration from. Hopefully you find some ideas here.


## features
- [Miryoku](https://github.com/manna-harbour/miryoku) keyboard layout
- broken actions build
- Mouse emulation (local build only)
- [urob's timeless homerow mods](https://github.com/urob/zmk-config#timeless-homerow-mods) (`config/includes`)

## local builds 
Building locally is much faster for testing and debugging. Paths in `local-build.sh` are hardcoded.

Requirements:
- initialize submodule: zmk-nodefree-config (`git submodule init`)
- [zmk toolchain](https://zmk.dev/docs/development/setup)
- install zephyr-sdk (likely to `~/.local/zephyr-sdk`)
- checkout zmk src to mousekeys PR

Use venv (ex. conda) of your choice to isolate west (I use the same one for qmk and west).
```bash
ZMK_HEAD=$(pwd)

# clone repo and initialize submodules
git@github.com:nickchahley/miryoku-zmk-swoop.git && cd miryoku-zmk-swoop
git submodule init

# install west into a venv
conda activate qmk
pip install --user -U west
west --version # verify installed

# install zephyr sdk
cd ~/.local
wget https://github.com/zephyrproject-rtos/sdk-ng/releases/download/v0.16.3/zephyr-sdk-0.16.3_linux-x86_64.tar.xz
wget -O - https://github.com/zephyrproject-rtos/sdk-ng/releases/download/v0.16.3/sha256.sum | shasum --check --ignore-missing
cd zephyr-sdk-0.16.3
./setup.sh

# back to wherever zmk repos are
cd "$ZMK_HEAD"

# setup zmk src
git clone https://github.com/zmkfirmware/zmk.git && cd zmk
west init -l app/
west update
west zephyr-export
pip3 install --user -r zephyr/scripts/requirements.txt

# checkout mouskeys PR (assuming zmk src repo is one above)
git fetch origin pull/2027/head:mousekeyspr
git switch mousekeyspr
```

