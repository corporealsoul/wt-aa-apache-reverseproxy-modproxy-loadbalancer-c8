### On Load Balancer Server, 192.168.0.200

### Install apache,

`[root@localhost ~]# sudo yum install httpd`

<br>

`[root@localhost ~]# sudo firewall-cmd --permanent --add-service=http`

`[root@localhost ~]# sudo firewall-cmd --permanent --add-service=https`

`[root@localhost ~]# sudo firewall-cmd --reload`

<br>

`[root@localhost ~]# httpd -v`

`[root@localhost ~]# apachectl configtest`

<br>

`[root@localhost ~]# vi /etc/hosts`

    127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
    ::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
    
    172.30.255.200 localhost.localdomain
    172.30.255.199 localhost.localdomain

`[root@localhost ~]# vi /etc/httpd/conf.modules.d/00-proxy.conf`

`[root@localhost ~]# vi /etc/httpd/conf.d/loadbalancer.conf`

    ProxyRequests off
    ProxyPreserveHost On
    
    <Proxy balancer://testload>
    BalancerMember http://172.30.255.199:4201
    BalancerMember http://172.30.255.199:4202
    BalancerMember http://172.30.255.199:4203
    ProxySet lbmethod=byrequests
    ProxySet stickysession=JSESSIONID
    </Proxy>
    
    <Location /balancer-manager>
    SetHandler balancer-manager
    </Location>
    
    ProxyPass /balancer-manager !
    ProxyPass / balancer://testload/
    ProxyPassReverse / balancer://testload/

`[root@localhost ~]# sudo systemctl restart httpd`

**On you browser,** http://192.168.0.200/

<br>

### On Web server or App server — 192.168.0.199

### Install apache,

`[root@localhost ~]# sudo yum install httpd`

<br>

`[root@localhost ~]# sudo firewall-cmd --permanent --add-service=http`

`[root@localhost ~]# sudo firewall-cmd --permanent --add-service=https`

`[root@localhost ~]# sudo firewall-cmd --reload`

<br>

`[root@localhost ~]# httpd -v`

`[root@localhost ~]# apachectl configtest`

<br>

`[root@localhost ~]# vi /etc/hosts`

    127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
    ::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
    
    172.30.255.199 localhost.localdomain
    172.30.255.200 localhost.localdomain

<br>

`[root@localhost ~]# vi /var/www/html/index.html`

    Web App Here . . !

http://192.168.0.199/

<br>

### Test on app server - 192.168.0.199

http://172.30.255.199/

http://172.30.255.199:4201/

http://172.30.255.199:4202/

http://172.30.255.199:4203/

<br>

### Test on Loadbalancer Server - 192.168.0.200

http://172.30.255.200/

<br>
