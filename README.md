Mail
====

A proof of concept for a webmail client with Meteor. Only some of the features have been implemented, and there is no security. Use at your own risk!

Install
=======

SMTP Server
-----------

Mail doesn't attempt to reinvent the wheel, and relies on a SMTP server for sending and receiving emails. You will need to have a working mail configuration first. Postfix is recommended - see for example the [Debian Postfix HOWTO][].

See also the _Incoming emails_ section below, to feed incoming emails into Mail.

Installation
------------

Install the dependencies first - for example, for a Debian-based system:

    $ sudo apt-get install python-daemon mongodb python-pymongo mongodb-clients mongodb-server

Install npm:

    $ curl http://npmjs.org/install.sh | sh

You will need to get the source of Meteor, rather than the official package, and add the nodemailer to the node modules:

    $ git clone https://github.com/meteor/meteor.git
    $ cd meteor
    $ ./install.sh
    $ cd dev_bundle/lib
    $ npm install nodemailer

Then clone the source repository of Mail and start it:

    $ git clone git@github.com:antoviaque/mail.git
    $ cd mail/app
    $ MONGO_URL=mongodb://localhost/mail ../../meteor/meteor run -p 4000 --production

Mail should now be accessible, by pointing your browser at http://localhost:4000/

Incoming emails
---------------

Mail receives incoming emails from the SMTP server through the LMTP protocol, a subset of the SMTP protocol.

First, start the LMTP daemon:

    $ cd /path/to/mail/
    $ cd lmtp
    $ ./lmtpd.py

Now, you need to configure your SMTP server, to get him to deliver the incoming emails to Mail rather than on a local mailbox. For example with Postfix, add the following to `/etc/postfix/main.cf`:

    virtual_transport = lmtp:127.0.0.1:1111
    virtual_mailbox_domains = maildev.plebia.org

This will feed emails addressed to any address like @maildev.plebia.org to Mail.

Also, make sure the domain name(s) you use for `virtual_mailbox_domains` aren't in `mydestination = ` in `main.cf`.

Troubleshooting
---------------

If you use Mail in development mode, and it refuses to start with the following message `throw errnoException(errno, 'watch');`, you need to increase the number of allowed instances from inotify (Linux):

    # echo 8192 > /proc/sys/fs/inotify/max_user_instances

License
-------

Copyright (C) 2012 Xavier Antoviaque <xavier@antoviaque.org>

This software's license gives you freedom; you can copy, convey,
propagate, redistribute and/or modify this program under the terms of
the GNU Affero General Public License (AGPL) as published by the Free
Software Foundation (FSF), either version 3 of the License, or (at your
option) any later version of the AGPL published by the FSF.

This program is distributed in the hope that it will be useful, but
WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU Affero
General Public License for more details.

You should have received a copy of the GNU Affero General Public License
along with this program in a file in the toplevel directory called
"AGPLv3".  If not, see <http://www.gnu.org/licenses/>.


[Debian Postfix HOWTO]:     http://wiki.debian.org/Postfix

