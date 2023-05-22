# Steam Integration

GOG Galaxy 2.0 Community integration for Steam.

## Open Beta:
This project is in open beta. It is not completely bulletproof, but it has been extensively tested. We are hoping that crowdsourcing the error checking will help find anything we may have missed.

### Known Issues:
* Large libraries are known to be a little wonky. Sometimes, the plugin may crash after initially starting it, but then immediately work properly once you hit "retry". There's not much we can do here.
* If you take too long to enter a 2FA code, any login stuff after that have a small chance of crashing. This is because steam kicks us out after roughly a minute of inactivity. We reconnect, but if we were sending them something when they kicked us out, we'd never get a response and after a minute of waiting, the plugin will close. Restarting the plugin will fix this, but it is something we are looking at fixing in a later version. In our tests, it has happened, but is difficult to replicate. If this issue is more prevalent than we anticipate, it's something we will address immediately. 
* Closing GOG immediately after connecting for the first time may cause GOG to crash. Similarly, disconnecting Steam immediately after connecting it may cause the plugin to crash, though you usually can hit retry and it'll work again. Basically, GOG thinks we are immediately done syncing once we connect, and that is not the case, especially for larger libraries. So, it thinks it can disconnect immediately too, and if we're in the middle of writing some data down, it has to wait, but has basically hamstrung us in the process. It's not an ideal situation. Fortunately, it only happens once, and won't happen at all if you give it time.

### Installation:
* There is a zip file in the releases directory. Download this.
* Extract the contents to the location where GOG stores the plugins. 
  * For windows, this is `%localappdata%\GOG.com\Galaxy\plugins\installed\`
  * For MacOS, this is `~/Library/Application Support/GOG.com/Galaxy/plugins/installed`
* Start GOG Galaxy. 

### Logging: 
We tried to kill as many bugs and test as many behaviors as possible, but we aren't perfect. You may find some case we haven't tested. Please raise an issue here, and in the comment, attach your logs. They can be found here: 
* For windows, this is `%programdata%\GOG.com\Galaxy\logs`
* For MacOS, this is `/Users/Shared/GOG.com/Galaxy/Logs`

We typically only need the `steam_<numbers and letters>.log` file.

## Setup (For Developers)
* Download Python 3.7.9 32-bit
* Install it with the defaults
* Create a new virtual env:
    - If you only have python 3.7.9<br/>
    `python -m venv .venv`
    - IF you have multiple python versions installed (assumes you have `py` as well)<br/>
    `py -3.7 -m venv .venv`
* Activate the virtual env 
  - Windows, Powershell:<br/>
  `.\.venv\Scripts\activate.ps1`
  - MacOS, terminal:<br/>
  `.venv/Scripts/activate
* Downgrade/Upgrade `pip`<br/>
    - If you only have python 3.7.9<br/>
    `python -m pip install --upgrade pip==22.0.4`
    - IF you have multiple python versions installed (assumes you have `py` as well)<br/>
    `py -m pip install --upgrade pip==22.0.4`
* Install the dev dependencies:<br/>
  `pip install -r requirements/dev.txt`
* Make your edits
* Update the protobufs (See README_UPDATE_PROTOBUF_FILES.md for more info)
  Take notice of the initial diff between the files in `protobuf_files` and `protobuf_files/orig`
  Generating the python files is done via:
  `inv GenerateProtobufMessages`
* Build your edits:
  `inv build`
* Test your edits:
  `inv test`
* Install your edits for a local test:
  `inv install`
* Build a release package (zip):
  `inv pack`

This is a fork of the repository from FriendsOfGalaxy, intended to continue development until they resume their work.

**This is unofficial and purely maintained by fans!**

## Installation (non-developers)

*~~The latest release should be available for download via the "Connect" button in Galaxy~~*
We aren't ready to publish this project to Galaxy just yet. We have the tools to do so, but the code is not stable enough for us to consider that just yet. 

In the meantime, we've provided a simplified version of the developer install process that only does the bare minimum to install the plugin. There are only a few commands you need to run, but if you want to know what they do, they are documented above each command. A tl;dr: version is below it. Please do the following:
* Download or clone this repo. If you download a zip, make sure to extract it. You need to be in the main directory for this to work. 
* Download Python 3.7.9 32-bit. If you have another version of python installed, make sure `install py` is checked. This makes it easier to select which version of python you are using and we need our virtual environment in 3.7.9. Also, make sure you have the setting that adds python to the path environmental variable checked (windows). These should be the default, but make sure anyway.
* Create a new virtual env:
    - If you only have python 3.7.9<br/>
    `python -m venv .venv`
    - IF you have multiple python versions installed (assumes you have `py` as well)<br/>
    `py -3.7 -m venv .venv`
* Activate the virtual env 
  - Windows, Powershell:<br/>
  `.\.venv\Scripts\activate.ps1`
  - MacOS, terminal:<br/>
  `.venv/Scripts/activate.sh`
* Downgrade/Upgrade `pip`<br/>
    - If you only have python 3.7.9<br/>
    `python -m pip install --upgrade pip==22.0.4`
    - IF you have multiple python versions installed (assumes you have `py` as well)<br/>
    `py -3.7 -m pip install --upgrade pip==22.0.4`
* Use Pip to get the python tools we need to install the plugin. These will only be applied to the venv you created earlier:<br/>
  `pip install -r requirements/install.txt`
* Install the plugin. It should work if you have deleted the original plugin, but will patch it if it is there.<br/>
  `inv install`

### Installation (non-dev, Tl;Dr):

<b>Windows (Powershell recommended)</b>
```
echo I have installed python 3.7.9 (32 bit). If not, the rest of this won't work.
py.exe -3.7 -m venv .venv
echo if the previous command did not work, you do not have py installed or py is not in your PATH. If you only have python 3.7.9, run the next command. If it worked, skip the next command.
python.exe -m venv .venv
echo virtual environment is installed. on to the next step.
.\.venv\Scripts\activate.ps1
echo i am downgrading pip.
py.exe -m pip install --upgrade pip==22.0.4
echo if the previous command did not work, you do not have py installed or py is not in your PATH. If you only have python 3.7.9, run the next command. If it worked, skip the next command.
python.exe -m pip install --upgrade pip==22.0.4
pip install -r requirements/install.txt
inv install
```

<b>MacOS</b> (assumes your shell is bash, which is the default. if you are good enough to change that, you can figure out how to run these)
```
echo I have installed python 3.7.9 (MacOS). If not, the rest of this won't work.
py -3.7 -m venv .venv
echo if the previous command did not work, you do not have py installed or py is not in your PATH. If you only have python 3.7.9, run the next command. If it worked, skip the next command.
python -m venv .venv
echo virtual environment is installed. on to the next step.
./.venv/Scripts/activate.sh
echo i am downgrading pip.
py -m pip install --upgrade pip==22.0.4
echo if the previous command did not work, you do not have py installed or py is not in your PATH. If you only have python 3.7.9, run the next command. If it worked, skip the next command.
python -m pip install --upgrade pip==22.0.4
pip install -r requirements/install.txt
inv install
```

### Install Error fixes:
If `inv install` throws a bunch of errors, make sure you have the proper python venv set up. It should complain about `getargspec`. If this happens, you created the wrong virtual environment. You can either delete .venv and reinstall it, or create a new virtual environment with a different name and use that. make sure you use `py -3.7` when creating your venv. If you don't have py, get it. You can specify the full path to python 3.7 instead if you want, but that's harder to do and harder to explain here. 

## Why this fork?

Well, without being too complicated, Steam changed how they do authentication. We used to be able to use one call and magically get a lot of info we needed. But, if we're being honest, it was a little insecure, and it easy. While we were using it for legitimate purposes, not everyone else was, and one of Valve's greatest deterrence to botting or DOS attacks (etc) is making things difficult. The new workflow uses a significant back-and-forth between a Steam Server and ourselves, closely resembling a common web form of authentication called OAuth2. This meant a lot of under-the-hood changes. 

## Credits

### Current Version:
This is a fork of https://github.com/FriendsOfGalaxy/galaxy-integration-steam

The new Authorization flow implementation is heavily influenced by SteamKit. https://github.com/SteamRE/SteamKit<br/>
While we have not utilized their source code, they have implemented the new authentication workflow before we did, and we used their knowledge of how to do so in order to implement it ourselves. If you are doing anything steam related in C#, you should check them out; their project has far more features than our own.

Some work was influenced by ValvePython. https://github.com/ValvePython/steam<br/>
Our projects do the same thing, but use different methods (we use asyncio, they use gevent, for example). Both projects were working on the new Auth Flow simultaneously, with little collaboration between us. That said, their scope is much larger than our own and lets you do a lot more things. If you are looking for a python means of implementing a steam network authentication, you should use their work instead.

### The names of individual developers will appear here, soon(ish). Any thanks can be directed there

### Original Version:

Original Plugin was based on work and research done by others:
* https://github.com/prncc/steam-scraper
* https://github.com/rhaarm/steam-scraper
* https://github.com/mulhod/steam_reviews
* https://github.com/summersb92/aeolipile
* https://github.com/rcpoison/steam-scraper
* https://github.com/chmccc/steam-scraper
