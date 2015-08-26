# nullmailer-smtpfix

Make SMTP servers happy to accept local generated email by cron and nullmailer

## The problem

Real SMTP servers require fully compilant "eletronic mail text message" (aka [https://tools.ietf.org/html/rfc2822](RFC 2822)), but old versions of cron don't honor `MAIL_FROM/MAIL_TO` crontab directives, neither some versions of nullmailer sets `From:` and `To:` mail headers from these.

You email don't leave your server, leaving errors as `Failed: 504 5.5.2 <user@host>: Sender address rejected: need fully-qualified address` from the receiving SMTP endpoint.

## The solution

This script can transform specific `X-Cron-Env:` headers into `From:` and `To:` RFC 2822 headers.

For older emails in spool (or very old cron and nullmailer versions), it's possible to force specific `From:/To:` headers directly from script.

## How to use

1. Change to nullmailer's [http://www.troubleshooters.com/linux/nullmailer/landmines.htm#_nullmailer_mental_model]("format executable") for MTA dir. Most likely `/usr/lib/nullmailer`.
2. Download `smptfix` from [https://raw.githubusercontent.com/alfsb/nullmailer-smtpfix/master/smptfix](from here).
3. Set up the same user & mod bits from `stmp`.
4. Change nullmailer's `remotes` file, replacing first `stmp` in each line by `stmpfix`.
5. Profit.

## How to debug

Change the script to have `$debug = true;` and as root run `nullmailer-send`.

## BE CAREFUL

Replacing nullmailse's default executables may cause your mail to be lost, your dog to fell ill and your house to get fire.

## If you are brave

```bash
sudo bash
cd /usr/lib/nullmailer
wget https://raw.githubusercontent.com/alfsb/nullmailer-smtpfix/master/smptfix
chown --reference=smtp smtpfix
chmod --reference=smtp smtpfix
nano /etc/nullmailer/remotes
```

