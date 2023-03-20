# hsk

The `hsklogin` script uses the macOS ssh-agent with the yubikey, 
so that you don't have to type in the PIN for every git pull.

## Prerequirements

Set up your yubikey for github SSH and test that it works correctly 
(follow instructions at https://w3.ibm.com/#/support/article/hsk/github).


## Installing

```
git clone https://github.com/mrszymon/hsk
cd hsk
sudo cp hsklogin /usr/local/bin
chmod u+x /usr/local/bin/hsklogin
```

## Usage

display installed keys:
```
hsklogin -l
hsklogin -L
```

create keys using default life time:
```
hsklogin
```

create keys with an explicit life time (in seconds):
```
hsklogin -t 6000
```

delete installed keys:
```
hsklogin -e
```


## Further information

You will be prompted for your yubikey PIN:
- if keys are not installed
- if installed keys have expired
- following a reboot

`hsklogin` can be executed multiple times. Keys are replaced on each execution.

The script performs the following steps:
- `ssh-add -e /usr/local/lib/libykcs11.dylib`
  - removes any existing keys, with a specified duration (in seconds)
- `ssh-add -s /usr/local/lib/libykcs11.dylib -t $life`
  - prompts for your Yubikey PIN
  - adds Yubikey keys
- `ssh-add -l`
  - lists the installed keys

For more information, see `man ssh-add`.

`hsklogin` was approved for use in the [#hsk-community])
(https://ibm-security.slack.com/archives/C025SQRFM6H/p1666714770856989) 
slack channel.

