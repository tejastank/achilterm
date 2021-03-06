# Web Based Terminal Access Control Solution, SnippetBucket.com

Written in python (and some AJAX javascript for client side) and depends only on python2.5 or better.

**very simple to install** on Linux, MacOS X, FreeBSD, Solaris, cygwin and any Unix that runs python2.5.

Support & Implementation Powered By SnippetBucket.com
Contact us: snippetbucket@gmail.com
Skype: live.snippetbucket


## Download and Install

To install Achilterm use following commands:
```
git clone https://github.com/tejastank/achilterm.git
cd achilterm
./achilterm.py
```
Then point your browser to this URL : `http://localhost:8022/`

## Documentation and Caveats

 * Currently it only support latin1, if you use Ubuntu or any LANG==en_US.UTF-8 distribution don't forget to "unset LANG".

 * If run as root will run /bin/login, otherwise it will run ssh
   localhost. To use an other command use the -c option.

 * By default Achilterm only listen at `127.0.0.1:8022`. For remote access, it is
   strongly recommended to use **https SSL/TLS**, and that is simple to
   configure if you use the apache web server using mod_proxy.

   Using ssl will also speed up achilterm (probably because of keepalive).

 * Using GET HTTP request seems to speed up achilterm, just click on GET in the
   interface, but be warned that your keystrokes might be loggued (by apache or
   any proxy). I usually enable it after the login.


## Configuration example

```
    Listen 443
    NameVirtualHost *:443

    <VirtualHost *:443>
       ServerName localhost
       SSLEngine On
       SSLCertificateKeyFile ssl/apache.pem
       SSLCertificateFile ssl/apache.pem

       ProxyRequests Off
       <Proxy *>
               Order deny,allow
               Allow from all
       </Proxy>
       ProxyPass /achilterm/ http://localhost:8022/
       ProxyPassReverse /achilterm/ http://localhost:8022/
    </VirtualHost>
```

## Commandline usage:

```
usage: achilterm.py [options]

options:
  -h, --help            show this help message and exit
  -pPORT, --port=PORT   Set the TCP port (default: 8022)
  -cCMD, --command=CMD  set the command (default: /bin/login or ssh localhost)
  -l, --log             log requests to stderr (default: quiet mode)
  -d, --daemon          run as daemon in the background
  -PPIDFILE, --pidfile=PIDFILE
                        set the pidfile (default: /var/run/achilterm.pid)
  -iINDEX_FILE, --index=INDEX_FILE
                        default index file (default: achilterm.html)
  -uUID, --uid=UID      Set the daemon's user id
```

## Compared to anyterm:

 * There are no partial updates, achilterm updates either all the screen or
   nothing. That make the code simpler and I also think it's faster. HTTP
   replies are always gzencoded. When used in 80x25 mode, almost all of
   them are below the 1500 bytes (size of an ethernet frame) and we just
   replace the screen with the reply (no javascript string handling).

 * Achilterm polls the server for updates with an exponentially growing
   timeout when the screen hasn't changed. The timeout is also resetted as
   soon as a key is pressed. Anyterm blocks on a pending request and use a
   parallel connection for keypresses. The anyterm approch is better
   when there aren't any keypress.


Don't worry much, its AGPLV3.0, Free Download, Free Use.
