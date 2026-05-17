# Wine
This a proton wine build that supports file creation times, which enables sorting by download date to work correctly inside Beat Saber using BetterSongList plugin.

Normally wine uses unix compatible methods for getting file info, but those do not support file creation time, this modification forces wine to use linux statx to get the file timestamps, which also provides file creation time.

demo:
![Wine comparison](https://github.com/ArttuKuikka/wine/blob/proton_11.0/winecomparison.png?raw=true)

# Building proton 11.0 (with modified wine)

> I use Kubuntu 26.04 for this, but it should work other distros as well, since proton is built inside a container(Docker in this case)

- Clone or download proton ```git clone --recurse-submodules https://github.com/ValveSoftware/Proton.git proton```

- switch to proton_11 ```git checkout proton_11.0```

- if branch was not already proton_11.0, update submodules ```git submodule update --init --recursive```

- cd into it ```cd proton```

- replace the wine directory inside proton folder with this repo. (Download this as zip and replace the folder)

- create build directory ```mkdir build && cd build```

- set environment variables ```DOCKER_OPTS="--security-opt seccomp=unconfined --security-opt apparmor=unconfined -e XDG_CACHE_HOME=/tmp/fontconfig-cache --tmpfs /tmp/fontconfig-cache:rw,exec,mode=1777 -v /etc/machine-id:/etc/machine-id:ro"```

- run configure and spesify build name ```../configure.sh --enable-ccache --build-name=bsproton``` (for errors, refer to readme in proton repo)

- run ```make install```

- (BSManager reguires this) create a link to wine from wine64 inside the folder where proton was installed. In my case cd ```cd /home/$(whoami)/.steam/debian-installation/compatibilitytools.d/bsproton/files/bin/ && ln -s wine wine64```

- Change proton folder to the one just created in BSManager settings, should be somethis like this ```/home/$(whoami)/.steam/debian-installation/compatibilitytools.d/bsproton```

- Done. Beat saber should now launch and sorting should work correctly. bsproton should also show in steam under compability and can be used from there if BSManager is not used
