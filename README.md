# nullmailer-smtpfix

Make SMTP servers happy to accept local generated email from misconfigured (or very old) cron or nullmailer.

## The problem

Real SMTP servers require fully compilant "eletronic mail text message" (aka [RFC 2822](https://tools.ietf.org/html/rfc2822)), but old versions of cron don't honor `MAIL_FROM/MAIL_TO` directives, neither some versions of nullmailer sets `From:` and `To:` mail headers from these.

You email don't leave your server, getting errors as `Failed: 504 5.5.2 <user@host>: Sender address rejected: need fully-qualified address` from the receiving SMTP endpoint.

## The solution

This script can transform specific `X-Cron-Env:` headers into `From:` and `To:` RFC 2822 headers.

For older emails in spool (or very old cron and nullmailer versions), it's possible to force specific `From:` and `To:` headers directly from script.

## How to use

1. Change to nullmailer's ["format executable"](http://www.troubleshooters.com/linux/nullmailer/landmines.htm#_nullmailer_mental_model) MTA dir. Most likely `/usr/lib/nullmailer`.
2. Download `smptfix` from [from here](https://raw.githubusercontent.com/alfsb/nullmailer-smtpfix/master/smtpfix).
3. `chown` and `chmod ` this file using `stmp` as reference.
4. Change nullmailer's `remotes` file, replacing first `stmp` in each line by `stmpfix`.
5. Profit.

## How to debug

Change the script to have `$debug = true;` and as root run `nullmailer-send`.

## BE CAREFUL

Replacing nullmailse's default executables may cause your mail to be lost, your dog to fell ill and your house to get fire. See LICENSE.

## If you are brave

```bash
sudo bash
cd /usr/lib/nullmailer
wget https://raw.githubusercontent.com/alfsb/nullmailer-smtpfix/master/smtpfix
chown --reference=smtp smtpfix
chmod --reference=smtp smtpfix
nano /etc/nullmailer/remotes
```
