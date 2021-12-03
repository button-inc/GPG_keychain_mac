### gpg-agent setup

Need to setup gpg-agent first, on OSX I use keychain (it also does ssh-agent)

```shell
$ brew info keychain
keychain: stable 2.8.5
User-friendly front-end to ssh-agent(1)
https://www.funtoo.org/Keychain
/usr/local/Cellar/keychain/2.8.5 (7 files, 108.5KB) *
  Built from source on 2018-10-23 at 14:44:08
From: https://github.com/Homebrew/homebrew-core/blob/master/Formula/keychain.rb
==> Analytics
install: 267 (30 days), 841 (90 days), 3,910 (365 days)
install_on_request: 262 (30 days), 817 (90 days), 3,661 (365 days)
build_error: 0 (30 days)
```

### gpg passphrase in keychain

```shell
brew install gpg gpg2 pinentry-mac
```

```shell
mkdir -m 0700 ~/.gnupg
echo 'pinentry-program /usr/local/bin/pinentry-mac' | tee ~/.gnupg/gpg-agent.conf
pkill -TERM gpg-agent
```

Close and reopen shell.

### test gpg passphrase stored in keychain

Assuming you've already created or imported a key, select an identity to test:

```shell
$ gpg --list-keys
/Users/kosh/.gnupg/pubring.kbx
------------------------------
pub   rsa4096 2019-06-18 [SC]
      C577EB80271726F2C2B75728BC90B58A3E7FC375
uid           [ultimate] Koshatul <koshatul@users.noreply.github.com>
sub   rsa4096 2019-06-18 [E]
```

Test (replace koshatul@users.noreply.github.com with the identity of your certificate):

```shell
$ echo test | gpg -e -r koshatul@users.noreply.github.com | gpg -d
gpg: encrypted with rsa4096 key, ID 3AF58C6962796950, created 2019-06-18
      "Koshatul <koshatul@users.noreply.github.com>"
test
```

### additional steps for M1 apple silicon users

Homebrew on M1 mac changes the install directory of all of its installs. If still getting errors about `pinentry` I recomend the following changes to your `gpg-agent.conf` file:

#1. Find the location of `pinentry`

```shell
$ which -a pinentry
```

#2. add that line to the `gpg-agent.conf` file
```shell
pinentry-program <line>
```

#3. Comment out the existing `pinentry-program` line

#4. Kill the exist gpg-agent process
```shell
$ killall gpg-agent
```

#5. Restart gpg-agent
```shell
$ gpg-agent --daemon
```
