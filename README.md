# gsm-network-limesdr

The following outlines the step that I took to set-up a GSM Network using a LimeSDR Mini on a Ubuntu 18.04 system (host OS).

The config (.cfg) files can be found under the `config` folder.

## Installing the Necessary Components

The following instructions are adopted from [Cyberloginit](https://cyberloginit.com/2018/04/27/build-a-gsm-network-with-openbsc-osmobts-osmotrx-and-usrp-b210-on-a-single-pc.html).

The bare minimum to get a GSM station up and running requires the installation of these programs:
- [OsmoTRX](https://osmocom.org/projects/osmotrx/wiki/OsmoTRX)
- [OsmoBTS](http://osmocom.org/projects/osmobts/wiki)
- [OpenBSC](https://osmocom.org/projects/openbsc/wiki/OpenBSC)
- [LimeSuiteGUI](https://wiki.myriadrf.org/LimeSuiteGUI)

The aforementioned programs are under the Osmocom project focused on open source mobile communications.

First, run the following to update any outdated programs.
```
sudo apt update
sudo apt upgrade
```

Install the required dependencies.
```
sudo apt install libdbi-dev libdbd-sqlite3 libortp-dev build-essential libtool autoconf autoconf-archive automake git-core pkg-config libtalloc-dev libpcsclite-dev libpcap-dev

```

These projects will have to be built from source:
- libosmo-abis
- libosmocore
- libosmo-netif
- openbsc
- osmo-bts
- osmo-trx

Create a folder(I have chosen `Projects` to store all the required program repositories:
```
mkdir Projects
cd Projects
```

For each of these projects, follow these instructions:
```
git clone git://git.osmocom.org/[project name]
cd [project name]
autoreconf -fi
./configure
make
make check
sudo make install
sudo ldconfig -i
```

`./configure` informs you of any missing dependencies. Install any missing dependencies (Google is your friend) and run `./configure` again.

**Note**
For `openbsc`, do `cd openbsc/openbsc` instead of `cd openbsc`.
For `osmo-bts`, make sure to run `./configure --enable-trx` instead of `./configure`.

Install UHD, which is available on the Ubuntu repository.
```
sudo apt install uhd-host libuhd-dev
```

Finally, install LimeSuiteGUI following the instructions from [here](https://wiki.myriadrf.org/Installing_Lime_Suite_on_Linux).

With all the necessary components installed, we will need the configuration (.cfg) files. These will include setings for the GSM network that you are attempting to set up. The ones I have used can be found on the `config` folder of this repository.

Download the configuration files into a folder (named `.osmocom`) in the home directory. i.e. `~/.osmocom`.

Run the following commands to start each component. You will need one terminal window for each of these commands:
```
sudo osmo-trx-lms -C ~/.osmocom/osmo-trx-lms.cfg
sudo osmo-nitb -c ~/.osmocom/osmo-nitb.cfg 
sudo osmo-bts-trx -c /.osmocom/osmo-bts.cfg 
```

The GSM network should be up and running.

