# t2m - Twitter 2 Mastodon

A script to manage the forwarding of tweets from Twitter accounts to a Mastodon one.

# Installation

On debian/ubuntu:

    sudo apt-get install python-virtualenv

    virtualenv ve
    source ve/bin/activate

    # if you run with an old version of python 2.7 (Ubuntu 14.04 for example)
    # you'll need to run those, otherwise requests will break because it won't
    # be able to correctly verify the host of the https issuer
    # if you use python 3 you can ignore that
    pip install pyopenssl ndg-httpsclient pyasn1

    pip install -r requirements.txt

Then you need twitter API credentials. Following this tutorial https://python-twitter.readthedocs.io/en/latest/getting_started.html then create a `conf.yaml` file of this format:

    consumer_key: "..."
    consumer_secret: "..."
    access_token_key: "..."
    access_token_secret: "..."

The credentials for Mastodon are automatically generated at the first startup.

# Python 2/3 and one known bug

Compatible with both.

There is a known bug if you run python2 coming for the STL lib `mimetypes`:
JPEG images will be uploaded with the `.jpe` extension, this will break "going
on the exact url of the image" (will cause a download instead of a display).

This bug is fixed in python 3 so I would recommend running t2m with it.

# Usage

## One account

Forward for one account:

    ./t2m one twitter_account -m mastodon_account

This will forward all not already forwarded tweet (this can be up to 200) while
waiting 30 seconds between each toot. This will also remember the mastodon account (so you don't need to specify it again).

RT and tweets that starts with a "@" won't be forwarded.

You might want a finer control on your action, so you can do:

    ./t2m one twitter_account -m mastodon_account -n 10

To forward only 10 tweet (be careful: if you relaunch the command this will forward 10 other tweets that weren't already forwarded).

You can also mark the whole available tweet as "already seen" without forwarding them so they'll never be forwarded in the future by using this command:

    ./t2m one twitter_account -m mastodon_account -o

If you want to test your commands without forwarding you can simply uses the `-d` (or `--debug`) option:

    ./t2m one twitter_account -m mastodon_account -d
    ./t2m one twitter_account -m mastodon_account -n 10 -d

## Recommendation

In general, when I had a new account I look at its timeline, read how many tweets make sens then do:

    ./t2m one twitter_account -m mastodon_account -n <number of tweets>
    ./t2m one twitter_account -m mastodon_account -o

## Several accounts

To forward tweets for all accounts, simply run:

    ./t2m all

This is a good command to put inside a crontab.

To check all accounts that will be forwarded, do a:

    ./t2m list

You can also add an account directly without using the `one` command using:

    ./t2m add twitter_account mastodon_account


# Licence

Gplv3+
