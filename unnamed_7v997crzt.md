---
tags: []
created: 2020-07-15T14:16:17.589Z
modified: 2020-07-15T14:16:19.089Z
---
# msmtp - ArchWiki

[Source](https://wiki.archlinux.org/index.php/msmtp "Permalink to msmtp - ArchWiki")

[msmtp](https://marlam.de/msmtp/) is a very simple and easy to use SMTP client with fairly complete [sendmail](https://wiki.archlinux.org/index.php/Sendmail) compatibility. 

## Installing

[Install](https://wiki.archlinux.org/index.php/Install) the [msmtp](https://www.archlinux.org/packages/?name=msmtp) package. Additionally, [install](https://wiki.archlinux.org/index.php/Install) [msmtp-mta](https://www.archlinux.org/packages/?name=msmtp-mta), which creates a sendmail alias to msmtp. 

## Basic setup

Since msmtp version 1.8.6 you can place your user config either at `~/.msmtprc` or `$XDG_CONFIG_HOME/msmtp/config`. The following is an example of a msmtp configuration (the file is based on the per-user example file located at `/usr/share/doc/msmtp/msmtprc-user.example`; the system configuration file belongs at `/etc/msmtprc` and its corresponding example file is located at `/usr/share/doc/msmtp/msmtprc-system.example`). 

    ~/.msmtprc

    # Set default values for all following accounts.
    defaults
    auth           on
    tls            on
    tls_trust_file /etc/ssl/certs/ca-certificates.crt
    logfile        ~/.msmtp.log

    # Gmail
    account        gmail
    host           smtp.gmail.com
    port           587
    from           username@gmail.com
    user           username
    password       plain-text-password

    # A freemail service
    account        freemail
    host           smtp.freemail.example
    from           joe_smith@freemail.example
    ...

    # Set a default account
    account default : gmail

**Note:** If you are using SSL/TLS and receive a "Server sent empty reply" error message, see [\#Server sent empty reply](https://wiki.archlinux.org/index.php/msmtp#Server_sent_empty_reply).

The *user* configuration file must be explicitly readable/writeable by its owner or msmtp will fail: 

    $ chmod 600 ~/.msmtprc

To avoid saving the password in plain text in the configuration file, use *passwordeval* to launch an external program, or see the [\#Password management](https://wiki.archlinux.org/index.php/msmtp#Password_management) section below. This example using Gnu PG is commonly used to perform decryption of a password: 

     echo -e "password\n" | gpg --encrypt -o .msmtp-gmail.gpg # enter id (email...)

**Warning:** Most shells save command history(e.g. .bash\_history .zhistory). To avoid this, use gpg with shell stdin: `gpg --encrypt -o .msmtp-gmail.gpg -r <email> -`. The ending dash is not a typo, rather it causes gpg to use stdin. After running that snippet of code, type in your password, press enter, and press Control-d so gpg can encrypt your password.

    ~/.msmtprc

    passwordeval    "gpg --quiet --for-your-eyes-only --no-tty --decrypt ~/.msmtp-gmail.gpg"

## Using the mail command

To send mails using the `mail` command you must install the package [s-nail](https://www.archlinux.org/packages/?name=s-nail), which also provides the `mailx` command. You will also need to provide a `sendmail`-compatible MTA, either by installing [msmtp-mta](https://www.archlinux.org/packages/?name=msmtp-mta) (which symlinks `sendmail` to `msmtp`) or by editing `/etc/mail.rc` to set the sendmail path: 

    /etc/mail.rc

    set mta=/usr/bin/msmtp

A `.msmtprc` file will need to be in the home of every user who wants to send mail or alternatively the system wide `/etc/msmtprc` can be used. 

msmtp also understands aliases. Add the following line to the defaults section of msmtprc or your local configuration file: 

    /etc/msmtprc

    aliases               /etc/aliases

and create an aliases file in `/etc`

    /etc/aliases

    # Example aliases file

    # Send root to Joe and Jane
    root: joe_smith@example.com, jane_chang@example.com

    # Send everything else to admin
    default: admin@domain.example

## Test functionality

The account option (`--account=,-a`) tells which account to use as sender: 

    $ echo "hello there username." | msmtp -a default username@domain.com

Or, with the addresses in a file: 

    To: username@domain.com
    From: username@gmail.com
    Subject: A test

    Hello there.

    $ cat test.mail | msmtp -a default <username>@domain.com

**Tip:** If using Gmail you'll need to either 

* If you use two factor authentication: [create an app password](https://myaccount.google.com/apppasswords).
* Otherwise: [allow less secure apps](https://myaccount.google.com/lesssecureapps). (not recommended)

**Tip:** You can use *--read-envelope-from* instead of *-a default* to automatically chose account by *From:* field in message you are going to send.

## Cronie default email client

[![Tango-view-refresh-red.png](https://wiki.archlinux.org/images/2/28/Tango-view-refresh-red.png)](https://wiki.archlinux.org/index.php/File:Tango-view-refresh-red.png)**This article or section is out of date.**[![Tango-view-refresh-red.png](https://wiki.archlinux.org/images/2/28/Tango-view-refresh-red.png)](https://wiki.archlinux.org/index.php/File:Tango-view-refresh-red.png)

**Reason:** Arch uses [systemd/Timers](https://wiki.archlinux.org/index.php/Systemd/Timers) instead of cronie (Discuss in [Talk:Msmtp\#](https://wiki.archlinux.org/index.php/Talk:Msmtp))

To make [Cronie](https://wiki.archlinux.org/index.php/Cron#Cronie) use msmtp rather than sendmail, make sure [msmtp-mta](https://www.archlinux.org/packages/?name=msmtp-mta) is installed, or edit the `cronie.service` systemd unit: 

    /etc/systemd/system/cronie.service.d/msmtp.conf

    [Service]
    ExecStart=
    ExecStart=/usr/bin/crond -n -m '/usr/bin/msmtp -t'

Then you must tell cronie or msmtp what your email address is, either by: 

1. Add to `/etc/msmtprc`: 

      aliases /etc/aliases

 and create `/etc/aliases`: 

      your_username: email@address.com

— OR —.

* Add a `MAILTO` line to the crontab: 

      MAILTO=email@address.com

## Password management

Passwords for msmtp [can be stored](https://marlam.de/msmtp/msmtp.html#Authentication) in plaintext, encrypted files, or a keyring. 

### GNOME Keyring

Storing passwords in [GNOME Keyring](https://wiki.archlinux.org/index.php/GNOME_Keyring) is supported natively in msmtp. Setup the keyring as described on the linked wiki page and install [libsecret](https://www.archlinux.org/packages/?name=libsecret). Then, store a password by running: 

     secret-tool store --label=msmtp host smtp.your.domain service smtp user yourusername

That's all, now msmtp should find the password automatically. 

### GnuPG

The `password` directive may be omitted. In that case, if the account in question has `auth` set to a legitimate value other than `off`, invoking msmtp from an interactive shell will ask for the password before sending mail. msmtp will not prompt if it has been called by another type of application, such as [Mutt](https://wiki.archlinux.org/index.php/Mutt). For such cases, the `--passwordeval` parameter can be used to call an external keyring tool like [GnuPG](https://wiki.archlinux.org/index.php/GnuPG). 

To do this, set up [GnuPG](https://wiki.archlinux.org/index.php/GnuPG), including [gpg-agent](https://wiki.archlinux.org/index.php/GnuPG#gpg-agent) to avoid having to enter the password every time. Then, create an encrypted password file for msmtp, as follows. Create a secure directory with `700` permissions located on a [tmpfs](https://wiki.archlinux.org/index.php/Tmpfs) to avoid writing the unencrypted password to the disk. In that directory create a plain text file with the mail account password. Then, encrypt the file with your private key: 

    $ gpg --default-recipient-self -e /path/to/plain/password

Remove the plain text file and move the encrypted file to the final location, e.g. `~/.mail/.msmtp-credentials.gpg`. In `~/.msmtprc` add: 

    ~/.msmtprc

    passwordeval  "gpg --quiet --for-your-eyes-only --no-tty --decrypt ~/.mail/.msmtp-credentials.gpg"

Normally this is sufficient for a GUI password prompt to appear when, for example, sending a message from [Mutt](https://wiki.archlinux.org/index.php/Mutt). If gpg prompt for the passphrase cannot be issued, then start the [gpg-agent](https://wiki.archlinux.org/index.php/GPG#gpg-agent) before. A simple hack to start the agent is to execute a external command in your muttrc using the backtick `` command `` syntax. For example, you can put something like the following in your muttrc 

    muttrc

    set my_msmtp_pass=`gpg -d mypwfile.gpg`

Mutt will execute this when it starts, gpg-agent will cache your password, msmtp will be happy and you can send mail. 

**Note:** If you do this, you will have to restart mutt after gpg-agent clears the password to start sending emails again

An alternative is to place passwords in `~/.netrc`, a file that can act as a common pool for msmtp, [OfflineIMAP](https://wiki.archlinux.org/index.php/OfflineIMAP), and associated tools. 

## Miscellaneous

### Using msmtp offline

Although msmtp is great, it requires that you be online to use it. This isn't ideal for people on laptops with intermittent connections to the Internet or dialup users. Several scripts have been written to remedy this fact, collectively called msmtpqueue. 

The scripts are installed under `/usr/share/doc/msmtp/msmtpqueue`. You might want to copy the scripts to a convenient location on your computer, (`/usr/local/bin` is a good choice). 

Finally, change your MUA to use msmtp-enqueue.sh instead of msmtp when sending e-mail. By default, queued messages will be stored in `~/.msmtpqueue`. To change this location, change the `QUEUEDIR=$HOME/.msmtpqueue` line in the scripts (or delete the line, and export the QUEUEDIR variable in `.bash_profile` like so: `export QUEUEDIR="$XDG_DATA_HOME/msmtpqueue"`). 

When you want to send any mail that you've created and queued up run: 

    $ /usr/local/bin/msmtp-runqueue.sh

Adding `/usr/local/bin` to your PATH can save you some keystrokes if you're doing it manually. The README file that comes with the scripts has some handy information, reading it is recommended. 

### Vim syntax highlighting

The msmtp source distribution includes an `msmtprc` syntax-highlighting script for [Vim](https://wiki.archlinux.org/index.php/Vim), which is available at `/usr/share/vim/vimfiles/syntax/msmtp.vim`. The filetype is not detected automatically. The easiest way to enable it is by adding a [modeline](http://vimdoc.sourceforge.net/htmldoc/options.html#modeline) at the top or bottom of the file(s), i.e.: 

       # vim:filetype=msmtp

### Send mail with PHP using msmtp

Look for *sendmail\_path* option in your `php.ini` and edit like this: 

    sendmail_path = "/usr/bin/msmtp -C /path/to/your/config -t"

Note that you **can not** use a user configuration file (ie: one under ~/) if you plan on using msmtp as a sendmail replacement with php or something similar. In that case just create /etc/msmtprc, and remove your user configuration (or not if you plan on using it for something else). Also make sure it's readable by whatever you're using it with (php, django, etc...) 

From the msmtp manual: *Accounts defined in the user configuration file override accounts from the system configuration file. The user configuration file must have no more permissions than user read/write*

So it's impossible to have a conf file under ~/ and have it still be readable by the php user. 

To test it place this file in your php enabled server or using php-cli. 

    <?php
    mail("your@email.com", "Test email from PHP", "msmtp as sendmail for PHP");
    ?>

### OAUTH2 Authentication for Gmail

msmtp now [supports OAUTH2 for Gmail.](https://marlam.de/msmtp/news/msmtp-with-oauth2-for-gmail/)

#### the oauth2 hack

**Note:** This method may work, but uses outdated tools.

To use XOAUTH2 authentication with Gmail (see [official information](https://developers.google.com/gmail/imap/xoauth2-protocol)), you can install the [msmtp-oauth2](https://aur.archlinux.org/packages/msmtp-oauth2/)AUR package in AUR. The package does a small hack so that the plain authentication method will send the `AUTH XOAUTH2 password` instead of the `AUTH PLAIN ...`, effectively disabling plain authentication and enabling XOAUTH2. Your `msmtp` would be adapted as follows: 

    from your@gmail_login_email
    tls on
    tls_starttls on
    tls_certcheck off
    auth plain
    user any_thing_here
    passwordeval "get-gmail-token"

The `get-gmail-token` script can be found from the source files of the AUR package. See more information on [getmail link](https://www.bytereef.org/howto/oauth2/getmail.html) about how this works. And see [Gmail API quickstart](https://developers.google.com/gmail/api/quickstart/python) for instruction on registering a Gmail APP and authorizing it to access emails. 

#### a wrapper on oauth2.py

This is now the **recommended method**, using the [msmtp setting oauthbearer](https://marlam.de/msmtp/msmtp.html#Authentication) for authentication. 

Once you have your [Gmail API](https://developers.google.com/gmail/api) setup, you can implement the wrapper script [oauth2token](https://github.com/tenllado/dotfiles/tree/master/config/msmtp) (that employs [secret-tool](http://manpages.ubuntu.com/manpages/trusty/man1/secret-tool.1.html)) or an [adaptation of oauth2token](https://github.com/harriott/ArchBuilds/blob/master/jo/mail/oauth2tool.sh) (that employs [pass](https://www.passwordstore.org/)). 

An `msmtp` configuration would be adapted thus: 

    auth oauthbearer
    ...
    passwordeval <call_to_wrapper_script>

If you comment out the last line, `msmtp` will request you for the token that `oauth2.py` provides you, which is normally valid for one hour. 

## Troubleshooting

### Issues with TLS

If you see the following message: 

     msmtp: TLS certificate verification failed: the certificate hasn't got a known issuer

it probably means your tls\_trust\_file is not right. 

Just follow the [fine manual](https://marlam.de/msmtp/msmtp.html#Transport-Layer-Security). It explains you how to find out the server certificate issuer of a given smtp server. Then you can explore the `/usr/share/ca-certificates/` directory to find out if by any chance, the certificate you need is there. If not, you will have to get the certificate on your own. If you are using your own certificate, you can make msmtp trust it by adding the following to your **~/.msmtprc**: 

     tls_fingerprint <SHA1 (recommended) or MD5 fingerprint of the certificate>

If you are trying to send mail through GMail and are receiving this error, have a look at [this](http://www.mail-archive.com/msmtp-users@lists.sourceforge.net/msg00141.html) thread or just use the second GMail example above. 

If you are completely desperate, but are 100% sure you are communicating with the right server, you can always temporarily disable the cert check: 

    $ msmtp --tls-certcheck off

If you see the following message: 

     msmtp: TLS handshake failed: the operation timed out

You may be affected by this [bug](https://bugs.archlinux.org/task/44994). Recompile with "--with-ssl=openssl" (msmtp is compiled with GnuTLS by default). 

### Server sent empty reply

If you get a "server sent empty reply" error, this probably means the mail server doesn't support STARTTLS over port 587, but requires TLS over port 465. 

To let msmtp use TLS over port 465, add the following line to `~/.msmtprc`: 

    tls_starttls off

### Issues with GSSAPI

If you get the following error 

    GNU SASL: GSSAPI error in client while negotiating security context in gss_init_sec_context() in SASL library.  This is most likely due insufficient credentials or malicious interactions.

Try changing your auth setting to plain, instead of gssapi in your .msmtprc file [[1]](https://bbs.archlinux.org/viewtopic.php?id=138727): 

    auth plain
