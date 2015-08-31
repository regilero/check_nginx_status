# CHECK_NGINX_STATUS


Nagios check checking the nginx_status page report from nginx. Tracking Active connections processes, request per second,
 connections per seconds, Connections status.

Nginx Monitor for Nagios version 0.10
GPL licence, (c)2012 Leroy Regis

## Known Bugs:

You may need to adapt the nagios libraries path on the script (currently /usr/local/nagios/libexec), line 18 of the script. It as been reported on different places such as /usr/local/libexec/nagios or /usr/share/nagios/libexec.

Check also the issues list [here](https://github.com/regilero/check_nginx_status/issues)

## Graphing Results


This check is compatible with performance data analysis for passive monitoring (RRD based graphs). So the script output is available for graph tools.
@maethor made a **PNP4Nagios** template available in this Gist:

* https://gist.github.com/maethor/8714514

## Dependencies

 * Perl
 * the `utils.pm` nagios perl library. Either in a perl standard PATH (@see http://stackoverflow.com/a/2526809) or in the same directory as the script.

## Script Documentation:


    Usage: ./check_nginx_status.pl -H <host ip> [-p <port>] [-s servername] [-t <timeout>] [-w <WARN_THRESOLD> -c <CRIT_THRESOLD>] [-V] [-d] [-u <url>] [-U user -P pass -r realm]
    -h, --help
       print this help message
    -H, --hostname=HOST
       name or IP address of host to check
    -p, --port=PORT
       Http port
    -u, --url=URL
       Specific URL to use, instead of the default "http://<hostname or IP>/nginx_status"
    -s, --servername=SERVERNAME
       ServerName, (host header of HTTP request) use it if you specified an IP in -H to match the good Virtualhost in your target
    -S, --ssl
       Wether we should use HTTPS instead of HTTP
    --disable-sslverifyhostname
       Disable SSL hostname verification
    -U, --user=user
       Username for basic auth
    -P, --pass=PASS
       Password for basic auth
    -r, --realm=REALM
       Realm for basic auth
    -d, --debug
       Debug mode (show http request response)
    -m, --maxreach=MAX
       Number of max processes reached (since last check) that should trigger an alert
    -t, --timeout=INTEGER
       timeout in seconds (Default: 15)
    -w, --warn=ACTIVE_CONN,REQ_PER_SEC,CONN_PER_SEC
       number of active connections, ReqPerSec or ConnPerSec that will cause a WARNING
       -1 for no warning
    -c, --critical=ACTIVE_CONN,REQ_PER_SEC,CONN_PER_SEC
       number of active connections, ReqPerSec or ConnPerSec that will cause a CRITICAL
       -1 for no CRITICAL
    -V, --version
       prints version number
    
    Note :
      3 items can be managed on this check, this is why -w and -c parameters are using 3 values thresolds
      - ACTIVE_CONN: Number of all opened connections, including connections to backends
      - REQ_PER_SEC: Average number of request per second between this check and the previous one
      - CONN_PER_SEC: Average number of connections per second between this check and the previous one
    
## Examples: 

This one will generate WARNING and CRITICIAL alerts if you reach 10 000 or 20 000 active connection; or
100 or 200 request per second; or 200 or 300 connections per second:

    check_nginx_status.pl -H 10.0.0.10 -u /foo/nginx_status -s mydomain.example.com -t 8 -w 10000,100,200 -c 20000,200,300

This will generate WARNING and CRITICAL alerts only on the number of active connections (with low numbers for nginx):

    check_nginx_status.pl -H 10.0.0.10 -s mydomain.example.com -t 8 -w 10,-1,-1 -c 20,-1,-1

Theses two equivalents will not generate any alert (if the nginx_status page is reachable) but could be used for graphics:

    check_nginx_status.pl -H 10.0.0.10 -s mydomain.example.com -w -1,-1,-1 -c -1,-1,-1
    check_nginx_status.pl -H 10.0.0.10 -s mydomain.example.com

## Licence: GNU GPL v3

