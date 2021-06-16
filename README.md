### gpg-agent setup
Need to setup gpg-agent first, on OSX I use keychain (it also does ssh-agent)
```
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
```
brew install gpg gpg2 pinentry-mac
```

```
mkdir -m 0700 ~/.gnupg
echo 'pinentry-program /usr/local/bin/pinentry-mac' | tee ~/.gnupg/gpg-agent.conf
pkill -TERM gpg-agent
```

Close and reopen shell.

Test:
```
echo test | gpg -e -r <identity> | gpg -d
```