---
title: "Installing LAMP stack on Ubuntu 20.04 using Ansible in 2022"
datePublished: Thu Sep 29 2022 14:21:56 GMT+0000 (Coordinated Universal Time)
cuid: clqnfbysy000o08l444abchzh
slug: installing-lamp-stack-on-ubuntu-20-04-using-ansible-in-2022-5f2116a8dd4
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1703660258423/66352b16-60ff-4e50-9275-7a677b4f929b.jpeg

---

Setting Up LAMP Stack Using Ansible

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660255510/3ffb9b93-7c81-467c-8407-f584ca994019.png)

Structure of How and where files are created?

Configurations are written below here:

Use wisely.

### **lamp-pb.yaml**

— \-  
\- hosts: ubuntulatest  
 become: true  
 ignore\_errors: yes  
 gather\_facts: false  
 pre\_tasks:   
 — name: Install python2 for ansible  
 apt: pkg=python2 state=present  
 register: output  
 changed\_when: output.stdout != “”  
 — name: Gathering facts  
 setup:   
 roles:  
 — apache2  
 — mysql  
 — php   
 vars:   
 mysql\_root\_password: “mypass”

### **roles/apache2/files/index.php**

<img src\=”https://s3.amazonaws.com/media-p.slid.es/uploads/77212/images/1762786/ansible\_logo\_black\_square.png" style\=”height: 100px;width: 200px;margin: 0 auto;display: table;”></img\>  
<?php  
 phpinfo();  
?>

### **roles/apache/tasks/main.yaml**

— \-  
\- name: Install apache2 web server  
 become: true  
 apt: pkg={{ item }} state=present update\_cache=true  
 with\_items:  
 — apache2

\- name: Enable apache2 modules  
 become: yes  
 ignore\_errors: yes  
 command: a2enmod rewrite actions alias fastcgi proxy\_fcgi

\- name: Removing existing index.html files  
 become: yes  
 ignore\_errors: yes  
 command: rm /var/www/html/index.html

\- name: Uploading own file  
 become: yes  
 copy: src=files/index.php dest=/var/www/html mode=0644

*   name: Restart apache2  
     become: yes  
     service: name=apache2 state=restarted

### **roles/mysql/tasks/main.yaml**

\- name: Add the repo for Mysql 8.0  
 become: yes  
 command: add-apt-repository ‘deb http://archive.ubuntu.com/ubuntu trusty universe’

\- name: Update  
 become: true  
 command: apt update

\- name: Install mysql  
 apt: pkg={{ item }} state=present  
 become: true  
 with\_items:   
 — mysql-server-8.0  
 — mysql-client-8.0  
 — python3-mysqldb  
 — libmysqlclient-dev

\- name: Ensure mysql started   
 become: yes  
 service: name=mysql state=started enabled=yes

name: Updating password   
 mysql\_user: name=root   
 host={{ item }}   
 password={{ mysql\_root\_password }}  
 login\_user=root  
 login\_password=””  
 state=present  
 with\_items:  
 — 127.0.0.1  
 — ::1  
 — localhost

### **roles/php/files/www.conf**

; Start a new pool named ‘www’.  
; the variable $pool can be used in any directive and will be replaced by the  
; pool name (‘www’ here)  
\[www\]

; Per pool prefix  
; It only applies on the following directives:  
; — ‘access.log’  
; — ‘slowlog’  
; — ‘listen’ (unixsocket)  
; — ‘chroot’  
; — ‘chdir’  
; — ‘php\_values’  
; — ‘php\_admin\_values’  
; When not set, the global prefix (or /usr) applies instead.  
; Note: This directive can also be relative to the global prefix.  
; Default Value: none  
;prefix = /path/to/pools/$pool

; Unix user/group of processes  
; Note: The user is mandatory. If the group is not set, the default user’s group  
; will be used.  
user = www-data  
group = www-data

; The address on which to accept FastCGI requests.  
; Valid syntaxes are:  
; ‘ip.add.re.ss:port’ — to listen on a TCP socket to a specific IPv4 address on  
; a specific port;  
; ‘\[ip:6:addr:ess\]:port’ — to listen on a TCP socket to a specific IPv6 address on  
; a specific port;  
; ‘port’ — to listen on a TCP socket to all addresses  
; (IPv6 and IPv4-mapped) on a specific port;  
; ‘/path/to/unix/socket’ — to listen on a unix socket.  
; Note: This value is mandatory.  
listen = 127.0.0.1:9000

; Set listen(2) backlog.  
; Default Value: 511 (-1 on FreeBSD and OpenBSD)  
;listen.backlog = 511

; Set permissions for unix socket, if one is used. In Linux, read/write  
; permissions must be set in order to allow connections from a web server. Many  
; BSD-derived systems allow connections regardless of permissions.  
; Default Values: user and group are set as the running user  
; mode is set to 0660  
listen.owner = www-data  
listen.group = www-data  
;listen.mode = 0660  
; When POSIX Access Control Lists are supported you can set them using  
; these options, value is a comma separated list of user/group names.  
; When set, listen.owner and listen.group are ignored  
;listen.acl\_users =  
;listen.acl\_groups =

; List of addresses (IPv4/IPv6) of FastCGI clients which are allowed to connect.  
; Equivalent to the FCGI\_WEB\_SERVER\_ADDRS environment variable in the original  
; PHP FCGI (5.2.2+). Makes sense only with a tcp listening socket. Each address  
; must be separated by a comma. If this value is left blank, connections will be  
; accepted from any ip address.  
; Default Value: any  
;listen.allowed\_clients = 127.0.0.1

; Specify the nice(2) priority to apply to the pool processes (only if set)  
; The value can vary from -19 (highest priority) to 20 (lower priority)  
; Note: — It will only work if the FPM master process is launched as root  
; — The pool processes will inherit the master process priority  
; unless it specified otherwise  
; Default Value: no set  
; process.priority = -19

; Choose how the process manager will control the number of child processes.  
; Possible Values:  
; static — a fixed number (pm.max\_children) of child processes;  
; dynamic — the number of child processes are set dynamically based on the  
; following directives. With this process management, there will be  
; always at least 1 children.  
; pm.max\_children — the maximum number of children that can  
; be alive at the same time.  
; pm.start\_servers — the number of children created on startup.  
; pm.min\_spare\_servers — the minimum number of children in ‘idle’  
; state (waiting to process). If the number  
; of ‘idle’ processes is less than this  
; number then some children will be created.  
; pm.max\_spare\_servers — the maximum number of children in ‘idle’  
; state (waiting to process). If the number  
; of ‘idle’ processes is greater than this  
; number then some children will be killed.  
; ondemand — no children are created at startup. Children will be forked when  
; new requests will connect. The following parameter are used:  
; pm.max\_children — the maximum number of children that  
; can be alive at the same time.  
; pm.process\_idle\_timeout — The number of seconds after which  
; an idle process will be killed.  
; Note: This value is mandatory.  
pm = dynamic

; The number of child processes to be created when pm is set to ‘static’ and the  
; maximum number of child processes when pm is set to ‘dynamic’ or ‘ondemand’.  
; This value sets the limit on the number of simultaneous requests that will be  
; served. Equivalent to the ApacheMaxClients directive with mpm\_prefork.  
; Equivalent to the PHP\_FCGI\_CHILDREN environment variable in the original PHP  
; CGI. The below defaults are based on a server without much resources. Don’t  
; forget to tweak pm.\* to fit your needs.  
; Note: Used when pm is set to ‘static’, ‘dynamic’ or ‘ondemand’  
; Note: This value is mandatory.  
pm.max\_children = 5

; The number of child processes created on startup.  
; Note: Used only when pm is set to ‘dynamic’  
; Default Value: min\_spare\_servers + (max\_spare\_servers — min\_spare\_servers) / 2  
pm.start\_servers = 2

; The desired minimum number of idle server processes.  
; Note: Used only when pm is set to ‘dynamic’  
; Note: Mandatory when pm is set to ‘dynamic’  
pm.min\_spare\_servers = 1

; The desired maximum number of idle server processes.  
; Note: Used only when pm is set to ‘dynamic’  
; Note: Mandatory when pm is set to ‘dynamic’  
pm.max\_spare\_servers = 3

; The number of seconds after which an idle process will be killed.  
; Note: Used only when pm is set to ‘ondemand’  
; Default Value: 10s  
;pm.process\_idle\_timeout = 10s;

; The number of requests each child process should execute before respawning.  
; This can be useful to work around memory leaks in 3rd party libraries. For  
; endless request processing specify ‘0’. Equivalent to PHP\_FCGI\_MAX\_REQUESTS.  
; Default Value: 0  
;pm.max\_requests = 500

; The URI to view the FPM status page. If this value is not set, no URI will be  
; recognized as a status page. It shows the following informations:  
; pool — the name of the pool;  
; process manager — static, dynamic or ondemand;  
; start time — the date and time FPM has started;  
; start since — number of seconds since FPM has started;  
; accepted conn — the number of request accepted by the pool;  
; listen queue — the number of request in the queue of pending  
; connections (see backlog in listen(2));  
; max listen queue — the maximum number of requests in the queue  
; of pending connections since FPM has started;  
; listen queue len — the size of the socket queue of pending connections;  
; idle processes — the number of idle processes;  
; active processes — the number of active processes;  
; total processes — the number of idle + active processes;  
; max active processes — the maximum number of active processes since FPM  
; has started;  
; max children reached — number of times, the process limit has been reached,  
; when pm tries to start more children (works only for  
; pm ‘dynamic’ and ‘ondemand’);  
; Value are updated in real time.  
; Example output:  
; pool: www  
; process manager: static  
; start time: 01/Jul/2011:17:53:49 +0200  
; start since: 62636  
; accepted conn: 190460  
; listen queue: 0  
; max listen queue: 1  
; listen queue len: 42  
; idle processes: 4  
; active processes: 11  
; total processes: 15  
; max active processes: 12  
; max children reached: 0  
;  
; By default the status page output is formatted as text/plain. Passing either  
; ‘html’, ‘xml’ or ‘json’ in the query string will return the corresponding  
; output syntax. Example:  
; [http://www.foo.bar/status](http://www.foo.bar/status)  
; [http://www.foo.bar/status?json](http://www.foo.bar/status?json)  
; [http://www.foo.bar/status?html](http://www.foo.bar/status?html)  
; [http://www.foo.bar/status?xml](http://www.foo.bar/status?xml)  
;  
; By default the status page only outputs short status. Passing ‘full’ in the  
; query string will also return status for each pool process.  
; Example:  
; [http://www.foo.bar/status?full](http://www.foo.bar/status?full)  
; [http://www.foo.bar/status?json&full](http://www.foo.bar/status?json&full)  
; [http://www.foo.bar/status?html&full](http://www.foo.bar/status?html&full)  
; [http://www.foo.bar/status?xml&full](http://www.foo.bar/status?xml&full)  
; The Full status returns for each process:  
; pid — the PID of the process;  
; state — the state of the process (Idle, Running, …);  
; start time — the date and time the process has started;  
; start since — the number of seconds since the process has started;  
; requests — the number of requests the process has served;  
; request duration — the duration in µs of the requests;  
; request method — the request method (GET, POST, …);  
; request URI — the request URI with the query string;  
; content length — the content length of the request (only with POST);  
; user — the user (PHP\_AUTH\_USER) (or ‘-’ if not set);  
; script — the main script called (or ‘-’ if not set);  
; last request cpu — the %cpu the last request consumed  
; it’s always 0 if the process is not in Idle state  
; because CPU calculation is done when the request  
; processing has terminated;  
; last request memory — the max amount of memory the last request consumed  
; it’s always 0 if the process is not in Idle state  
; because memory calculation is done when the request  
; processing has terminated;  
; If the process is in lamp-pb.yamlIdle state, then informations are related to the  
; last request the process has served. Otherwise informations are related to  
; the current request being served.  
; Example output:  
; \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*  
; pid: 31330  
; state: Running  
; start time: 01/Jul/2011:17:53:49 +0200  
; start since: 63087  
; requests: 12808  
; request duration: 1250261  
; request method: GET  
; request URI: /test\_mem.php?N=10000  
; content length: 0  
; user: -  
; script: /home/fat/web/docs/php/test\_mem.php  
; last request cpu: 0.00  
; last request memory: 0  
;  
; Note: There is a real-time FPM status monitoring sample web page available  
; It’s available in: /usr/share/php/7.0/fpm/status.html  
;  
; Note: The value must start with a leading slash (/). The value can be  
; anything, but it may not be a good idea to use the .php extension or it  
; may conflict with a real PHP file.  
; Default Value: not set  
;pm.status\_path = /status

; The ping URI to call the monitoring page of FPM. If this value is not set, no  
; URI will be recognized as a ping page. This could be used to test from outside  
; that FPM is alive and responding, or to  
; — create a graph of FPM availability (rrd or such);  
; — remove a server from a group if it is not responding (load balancing);  
; — trigger alerts for the operating team (24/7).  
; Note: The value must start with a leading slash (/). The value can be  
; anything, but it may not be a good idea to use the .php extension or it  
; may conflict with a real PHP file.  
; Default Value: not set  
;ping.path = /ping

; This directive may be used to customize the response of a ping request. The  
; response is formatted as text/plain with a 200 response code.  
; Default Value: pong  
;ping.response = pong

; The access log file  
; Default: not set  
;access.log = log/$pool.access.log

; The access log format.  
; The following syntax is allowed  
; %%: the ‘%’ character  
; %C: %CPU used by the request  
; it can accept the following format:  
; — %{user}C for user CPU only  
; — %{system}C for system CPU only  
; — %{total}C for user + system CPU (default)  
; %d: time taken to serve the request  
; it can accept the following format:  
; — %{seconds}d (default)  
; — %{miliseconds}d  
; — %{mili}d  
; — %{microseconds}d  
; — %{micro}d  
; %e: an environment variable (same as $\_ENV or $\_SERVER)  
; it must be associated with embraces to specify the name of the env  
; variable. Some exemples:  
; — server specifics like: %{REQUEST\_METHOD}e or %{SERVER\_PROTOCOL}e  
; — HTTP headers like: %{HTTP\_HOST}e or %{HTTP\_USER\_AGENT}e  
; %f: script filename  
; %l: content-length of the request (for POST request only)  
; %m: request method  
; %M: peak of memory allocated by PHP  
; it can accept the following format:  
; — %{bytes}M (default)  
; — %{kilobytes}M  
; — %{kilo}M  
; — %{megabytes}M  
; — %{mega}M  
; %n: pool name  
; %o: output header  
; it must be associated with embraces to specify the name of the header:  
; — %{Content-Type}o  
; — %{X-Powered-By}o  
; — %{Transfert-Encoding}o  
; — ….  
; %p: PID of the child that serviced the request  
; %P: PID of the parent of the child that serviced the request  
; %q: the query string  
; %Q: the ‘?’ character if query string exists  
; %r: the request URI (without the query string, see %q and %Q)  
; %R: remote IP address  
; %s: status (response code)  
; %t: server time the request was received  
; it can accept a strftime(3) format:  
; %d/%b/%Y:%H:%M:%S %z (default)  
; The strftime(3) format must be encapsuled in a %{<strftime\_format>}t tag  
; e.g. for a ISO8601 formatted timestring, use: %{%Y-%m-%dT%H:%M:%S%z}t  
; %T: time the log has been written (the request has finished)  
; it can accept a strftime(3) format:  
; %d/%b/%Y:%H:%M:%S %z (default)  
; The strftime(3) format must be encapsuled in a %{<strftime\_format>}t tag  
; e.g. for a ISO8601 formatted timestring, use: %{%Y-%m-%dT%H:%M:%S%z}t  
; %u: remote user  
;  
; Default: “%R — %u %t \\”%m %r\\” %s”  
;access.format = “%R — %u %t \\”%m %r%Q%q\\” %s %f %{mili}d %{kilo}M %C%%”

; The log file for slow requests  
; Default Value: not set  
; Note: slowlog is mandatory if request\_slowlog\_timeout is set  
;slowlog = log/$pool.log.slow

; The timeout for serving a single request after which a PHP backtrace will be  
; dumped to the ‘slowlog’ file. A value of ‘0s’ means ‘off’.  
; Available units: s(econds)(default), m(inutes), h(ours), or d(ays)  
; Default Value: 0  
;request\_slowlog\_timeout = 0

; The timeout for serving a single request after which the worker process will  
; be killed. This option should be used when the ‘max\_execution\_time’ ini option  
; does not stop script execution for some reason. A value of ‘0’ means ‘off’.  
; Available units: s(econds)(default), m(inutes), h(ours), or d(ays)  
; Default Value: 0  
;request\_terminate\_timeout = 0

; Set open file descriptor rlimit.  
; Default Value: system defined value  
;rlimit\_files = 1024

; Set max core size rlimit.  
; Possible Values: ‘unlimited’ or an integer greater or equal to 0  
; Default Value: system defined value  
;rlimit\_core = 0

; Chroot to this directory at the start. This value must be defined as an  
; absolute path. When this value is not set, chroot is not used.  
; Note: you can prefix with ‘$prefix’ to chroot to the pool prefix or one  
; of its subdirectories. If the pool prefix is not set, the global prefix  
; will be used instead.  
; Note: chrooting is a great security feature and should be used whenever  
; possible. However, all PHP paths will be relative to the chroot  
; (error\_log, sessions.save\_path, …).  
; Default Value: not set  
;chroot =

; Chdir to this directory at the start.  
; Note: relative path can be used.  
; Default Value: current directory or / when chroot  
;chdir = /var/www

; Redirect worker stdout and stderr into main error log. If not set, stdout and  
; stderr will be redirected to /dev/null according to FastCGI specs.  
; Note: on highloaded environement, this can cause some delay in the page  
; process time (several ms).  
; Default Value: no  
;catch\_workers\_output = yes

; Clear environment in FPM workers  
; Prevents arbitrary environment variables from reaching FPM worker processes  
; by clearing the environment in workers before env vars specified in this  
; pool configuration are added.  
; Setting to “no” will make all environment variables available to PHP code  
; via getenv(), $\_ENV and $\_SERVER.  
; Default Value: yes  
;clear\_env = no

; Limits the extensions of the main script FPM will allow to parse. This can  
; prevent configuration mistakes on the web server side. You should only limit  
; FPM to .php extensions to prevent malicious users to use other extensions to  
; exectute php code.  
; Note: set an empty value to allow all extensions.  
; Default Value: .php  
;security.limit\_extensions = .php .php3 .php4 .php5 .php7

; Pass environment variables like LD\_LIBRARY\_PATH. All $VARIABLEs are taken from  
; the current environment.  
; Default Value: clean env  
;env\[HOSTNAME\] = $HOSTNAME  
;env\[PATH\] = /usr/local/bin:/usr/bin:/bin  
;env\[TMP\] = /tmp  
;env\[TMPDIR\] = /tmp  
;env\[TEMP\] = /tmp

; Additional php.ini defines, specific to this pool of workers. These settings  
; overwrite the values previously defined in the php.ini. The directives are the  
; same as the PHP SAPI:  
; php\_value/php\_flag — you can set classic ini defines which can  
; be overwritten from PHP call ‘ini\_set’.  
; php\_admin\_value/php\_admin\_flag — these directives won’t be overwritten by  
; PHP call ‘ini\_set’  
; For php\_\*flag, valid values are on, off, 1, 0, true, false, yes or no.

; Defining ‘extension’ will load the corresponding shared extension from  
; extension\_dir. Defining ‘disable\_functions’ or ‘disable\_classes’ will not  
; overwrite previously defined php.ini values, but will append the new value  
; instead.

; Note: path INI options can be relative and will be expanded with the prefix  
; (pool, global or /usr)

; Default Value: nothing is defined by default except the values in php.ini and  
; specified at startup with the -d argument  
;php\_admin\_value\[sendmail\_path\] = /usr/sbin/sendmail -t -i -f [www@my.domain.com](mailto:www@my.domain.com)  
;php\_flag\[display\_errors\] = off  
;php\_admin\_value\[error\_log\] = /var/log/fpm-php.[www.log](http://www.log)  
;php\_admin\_flag\[log\_errors\] = on  
;php\_admin\_value\[memory\_limit\] = 32M

### **roles/php/tasks/main.yaml**

\- name: Install php-fpm & php extensions  
 become: true  
 apt: pkg={{ item }} state=present update\_cache=true  
 with\_items:  
 — php-fpm  
 — libapache2-mod-php  
 — php-gd  
 — php-curl  
 — php-mysql  
 — php-dom  
 — php-xml

\- name: Configure php-fom with apache2  
 become: true  
 copy: src=files/[www.conf](http://www.conf) dest=/etc/php/7.0/fpm/pool.d/

*   name: Restart php-fpm  
     become: yes  
     service: name=php7.4-fpm state=restarted

In the end, you need to run the playbook by present in the lamp-server directory where you have to use the below command:

**ansible-playbook lamp-server.yaml**

Also you can dry run the playbook by the below command:

**ansible-playbook lamp-server.yaml — — check**

You can refer to the github link where you just need to clone the repository.

I know I know I know , that you are in devOps. So you already knew about the Github. :)

[**GitHub - AmanPathak-dev/Ansible--Playbooks**  
*You can't perform that action at this time. You signed in with another tab or window. You signed out in another tab or…*github.com](https://github.com/AmanPathak-dev/Ansible--Playbooks.git "https://github.com/AmanPathak-dev/Ansible--Playbooks.git")[](https://github.com/AmanPathak-dev/Ansible--Playbooks.git)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1703660257158/07938b1c-e4a5-48db-81e7-33965864249c.jpeg)

I made your day, I know and Welcome

Author- Aman Pathak