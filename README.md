# CS:GO Community Server Materials Downloader

`CSMD` downloads community server's files based on a console log of file requests. It is created for Linux, but can be used on macOS or Windows if needed.

## Rationale

When one chooses a community server in a server list, the client basically executes `connect <ip:port>`, thus establishing communication and requesting any additional files that are missing from the client. For some unknown reason, a reason only Valve employee can know, Linux client refuses to download any content from FastDL type of servers. It has been like that for a very long time and the problem [has been reported](https://github.com/ValveSoftware/csgo-osx-linux/issues/11) to Valve Sep 23, 2014. Since the issue remained unsolved, I decided to create at least some workaround - this script.

## Installation

The script requires Python 3.

Since the script must not be run as root (as it will lock the downloaded files under root), execute this in a terminal exactly as written out as a regular user (i.e. not root)(this assumes `sudo` is configured correctly):

```
sudo curl -o /usr/local/bin/csgo-csmd https://raw.githubusercontent.com/M-Reimer/csgo-csmd/master/csgo-csmd
sudo chmod +x /usr/local/bin/csgo-csmd
```

Now configure your "Launch Options" for CS:GO to contain -condebug and -conclearlog

> __TIP__: If you want to download less bloat from the community servers, you might want to use `cl_downloadfilter` option in your `autoexec.cfg`. You can set it to "all", "none" or "nosounds" in order to not filter out any files, filter out all files and filter out sound files respectively. Example: `cl_downloadfilter "nosounds"`.

## Usage

1. Find a community server that your client refuses to connect to ("Downloading map <mapname> 0 % / 0 bytes").
2. Open CS:GO console (press `~` A.K.A. tilde, one has to have `-console` in the [game launch options](https://steamcommunity.com/sharedfiles/filedetails/?id=379782151) or `con_enable "1"` in [autoexec.cfg](http://dathost.net/blog/how-to-create-an-autoexec-cfg-for-csgo/)).
3. Type `disconnect` and press `Enter`.
4. Open a terminal (the one with a shell session) and run `csgo-csmd`.
5. Open CS:GO console again and type `retry`.
6. Have fun!

### Command-line arguments

```
usage: csgo-csmd [-h] [--config-path CONFIG_PATH] [--csgo-path CSGO_PATH]

optional arguments:
  -h, --help            show this help message and exit
  --config-path CONFIG_PATH
                        Path to CSMD config (default:
                        /home/$USER/.config/csmd/csmd.conf)
  --csgo-path CSGO_PATH
                        Path to CS:GO home folder (default:
                        /home/$USER/.local/share/Steam/steamapps/common/Counter-Strike Global Offensive/csgo)
```

## Configuration

CSMD provides a config in INI format (spaces between words are allowed). The default one is created upon the first invocation:

```ini
[path]
csgo_path = /home/$USER/.local/share/Steam/steamapps/common/Counter-Strike Global Offensive/csgo
```

* `csgo_path` can be anything `--csgo-path` accepts (see above)

## Bugs and feature requests

Open new tickets through [GitHub Issues](https://github.com/M-Reimer/csgo-csmd/issues). Please take time to describe the problem and copy the logs the script produces.

## Caveats

* CSMD doesn't know how to decompress anything except bzip2
* CSMD doesn't talk HTTPS (only HTTP)

If any of the above makes your life worse, open a new issue.

## License

You are allowed to share and modify all files in this repository under BSD 3-clause license. See [LICENSE](LICENSE) for more details.
