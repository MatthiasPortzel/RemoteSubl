# RemoteSubl

RemoteSubl starts as a fork of [rsub](https://github.com/henrikpersson/rsub) to
bring `rmate` feature of TextMate to Sublime Text. It transfers files to be
edited from remote server using SSH port forward and transfers the files back
when they are saved.

Comparing to rsub, the followings are enhanced:

- support multiple files via `rmate foo bar`.
- use the same view when opening the same file twice.
- notify when connection lost.
- resume previous connection when it was lost.
- better status bar messages when saving file and when encountering errors.
- bring up Sublime Text on different platforms.
- ability to set a custom color scheme.

Why a new fork? It seems that the author of rsub is not actively maintaining
that package.

# Installation

Before installing on your remote server, RemoteSubl can easily be installed
using [Package Control](https://packagecontrol.io).

Once you have that completed, open up Sublime Text *(the rest won't work unless
you do)*.

On the remote server, we need to install
[rmate](https://github.com/aurora/rmate) (this one is the bash version). You
don't have to install it if you have been using `rmate` with TextMate or other
editors. It is just the same executable. If not, it (the bash version) can be
installed by running this script (assuming you have the right permission),

```bash
curl -o /usr/local/bin/rmate https://raw.githubusercontent.com/aurora/rmate/master/rmate
sudo chmod +x /usr/local/bin/rmate
```

You can also rename the command to `rsubl`

```
mv /usr/local/bin/rmate /usr/local/bin/rsubl
```

If your remote system does not have `bash` (so what else does it have?), there
are different versions of `rmate` to choose from:

- The official ruby version: https://github.com/textmate/rmate
- A bash version: https://github.com/aurora/rmate
- A perl version: https://github.com/davidolrik/rmate-perl
- A python version: https://github.com/sclukey/rmate-python
- A nim version: https://github.com/aurora/rmate-nim
- A C version: https://github.com/hanklords/rmate.c
- A node.js version: https://github.com/jrnewell/jmate

# Usage

Open an ssh connection to the remote server with remote port forwarded. It can
be done by executing the following command on your local machine:

```bash
ssh -R 52698:localhost:52698 user@example.com
```

After running the server, you can just open the file by typing the following
command in your remote system's terminal:

```
rmate test.txt
```

(***NOTE:*** you need to have opened Sublime Text on your local machine.
*If not* you get this error: `connect_to localhost port 52698: failed.` on your
server)

... or if you renamed it to `rsubl` then ...

```
rsubl test.txt
```

If everything has been setup correctly, you should be able to see the opening
file in Sublime Text.

### SSH config

It could be tedious to type `-R 52698:localhost:52698` everytime you ssh. To
make your life easier, add the following to `~/.ssh/config`,

```
Host example.com
    RemoteForward 52698 localhost:52698
    User user
```

From now on, you only have to do `ssh example.com`.
