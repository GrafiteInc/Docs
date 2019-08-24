# Homeforge

!!! Warning
    This package is archived and no longer supported (2019-08-24)

It only seems sensible to have a console tool that can easily add and remove projects from Homestead rather than force us developers to manually edit files each time.

!!! Tip
    Supports OSX only (for now) - pull-requests are always welcome.

!!! Tip "Versions!!"
    Older versions of Homestead use the `.homestead` directory to provision from but the new version uses the `Homestead.yaml` file in the `~/Homestead` directory.
    If you're on homestead `4.x` then please use homeforge `0.2.x` otherwise you should use `0.3.x+`

## Installation

Installation should be done on the host system not the Homestead VM.

Make sure you have `~/.composer/vendor/bin/` in your PATH.

* Run the following command:

```bash
composer global require grafite/homeforge
```

## Commands

You can add domains to Homestead with one command:
Schedule does not take arguments and is optional.

```
homeforge site:list
homeforge site:add {domain name} {"path to public directory"} --schedule --database
homeforge site:remove {domain name}
homeforge site:share {domain name}
homeforge database:add {name}
homeforge database:remove {name}
```

## Sharing

Sharing requires `ngrok` to be installed on your system. Please visit [https://ngrok.com](https://ngrok.com/download) to download and install.

You also have to manually set some urls in your homestead config when you want to share. Please review the instructions provided by the share command.
