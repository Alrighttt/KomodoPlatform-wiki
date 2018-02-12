**This guide is intended for Unix based systems (Linux & MacOS) only using command line interface (CLI). GUI users, get [BarterDEX GUI](https://github.com/KomodoPlatform/BarterDEX). For Windows installations please refer to this [guide](https://github.com/DeckerSU/SuperNET/blob/dev-decker-dev/iguana/dexscripts.win32/how_to_use.md) and use this [link for the binaries](https://github.com/KomodoPlatform/BarterDEX/tree/v0.8/assets/bin/win64).**

Komodo Platform (barterDEX) is capable of working with Electrum servers. This means it is not necessary to download blockchain data in your computer. As a matter of fact, you don't need even download and run native coin wallet or daemon.
Check the list of current Electrum servers here: [Electrum Servers List](https://github.com/KomodoPlatform/KomodoPlatform/wiki/Electrum-servers-list).

If you however choose to use a native coin daemon, please make sure it is fully synced. For Komodo and other assetchains use the [latest Agama installer](https://artifacts.supernet.org/latest/) or use the [KomodoOcean-QT](https://github.com/DeckerSU/komodo-qt/releases).

For a list of currently supported native coins and instructions for the coin specific configuration file, see https://support.supernet.org/support/solutions/articles/29000014808-installation-script-for-various-coins

You may want to back up your system or clone it after everything is synced and running correctly BEFORE you start installing coin daemons.

### Installing Komodo Platform (barterDEX)

#### Install the following dependency packages:

```shell
sudo apt-get update
sudo apt-get install git libcurl4-openssl-dev build-essential
```

#### Install `nanomsg`

##### Linux:

```shell
cd ~
git clone https://github.com/nanomsg/nanomsg
cd nanomsg
cmake . -DNN_TESTS=OFF -DNN_ENABLE_DOC=OFF
make -j2
sudo make install
sudo ldconfig
```

##### MacOS
You'll need to install `homebrew` (Google how to do it)

```
brew install nanomsg
```

#### Clone the SuperNET repo from github & Install:
Clone the repo, checkout dev branch for latest and install.

```shell
cd ~
git clone https://github.com/jl777/SuperNET
cd ~/SuperNET/iguana/exchanges
git checkout dev
./install
```

#### Copy the `passphrase` file & using seed passphrase

From the same dir in terminal type the following commands to copy the `passphrase` file to `~/SuperNET/iguana/dexscripts` dir and add a strong 24 words seed passphrase in between `""`. Save your passphrase properly without changing any word or space. Same seed **passphrase** will always show you the same **smartaddress**.

```shell
cp passphrase ../dexscripts/passphrase
cd ../dexscripts
nano passphrase
```
Enter your passphrase:

`export passphrase="<put a very strong passphrase here>"`

Press `CTRL+X` then `Y` then `ENTER` to save the file and exit from Nano editor.

#### Getting the `userpass` value

All these scripts are expecting a `userpass` file, which contains the definition of the `$userpass` variable (found inside scripts) to authenticate API access. This avoids evil webpages that try to issue port 7783 calls to steal your money. At first you wont know the value of u`serpass`. To find out, just run the `client` script first (as instructed below) and then run `./setpassphrase` script 2 times. 2nd call will display your `userpass` field and you can copy that value and put it into `~/SuperNET/iguana/dexscripts/userpass` file. If you don't, all subsequent API calls will get authorisation error.

Open a new terminal and type the following:

```shell
cd ~/SuperNET/iguana/dexscripts
./client &
./setpassphrase
./setpassphrase
pkill -15 marketmaker
```
#### Edit the `userpass` file
Now copy the `userpass` example file to `~/SuperNET/iguana/dexscripts` dir and edit the file to save the `userpass` you got from the 2nd call of `./setpassphrase`

```shell
cd ~/SuperNET/iguana/exchanges
cp userpass ../dexscripts/userpass
cd ../dexscripts
nano userpass
```

Once done press `CTRL+X` then `Y` then `ENTER` to save the file and exit from Nano editor.

barterDEX is now installed in your system.

#### Running barterDEX

Every time you want to run **barterDEX** open a new terminal window and type the following:
```shell
cd ~/SuperNET/iguana/dexscripts
./client &
./setpassphrase
./setpassphrase
```
And, don't close it. Open a new terminal window to issue all other scripts/API calls from next. Get all available api list by typing `./help` inside `~/SuperNET/iguana/dexscripts` dir. You can see all scripts available for you to modify, test and use. Check the [barterDEX API Summary](https://github.com/KomodoPlatform/KomodoPlatform/wiki/BarterDEX-API-Summary-by-Category) for more info. Just run them in this terminal.

#### Stopping `killing marketmaker` / Close the app

If you want to close barterDEX, issue `pkill -15 marketmaker` every time. This ensures all BarterDEX process is killed safely.

#### Adding coins

You can run barterDEX and add coins without downloading any blockchain data using Electrum mode. Edit the `electrum` script with the list of servers you want to use. Native mode is faster and you need to use native wallet running with fully synced blockchain data and your barterDEX seed passphrase imported into the wallet. Edit the `enable` script and run it for adding coins native mode.