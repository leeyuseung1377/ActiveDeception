# Active-Deception
Making Active Decption system

Purpose of this system is trap attacker in honeypot(Fake Web) analize new attack pattern.

# BluePrint
VM 1 : Nginx, Suricata, Controller / IP : 192.168.85.100 / spec : 2 vCPU, 4GB 

VM 2 : Real Web / IP : 192.168.85.150 / spec : 2 vCPU, 4GB 

VM 3 : HoneyPot Web  / IP : 192.168.85.160 / spec : 2 vCPU, 4GB 

VM 4 : Elasticsearch, Kibana / IP : 192.168.85.105 / spec : 4 vCPU, 8GB 

# Software Components

The following components are used:

* Suricata 8.0.3
* Nginx
* Python 3
* systemd
* Kali Linux
* Real Web Server
* Honeypot Server

# The Suricata monitoring interface used in this environment is:
* enp2s0

The local Suricata rule file is:
* /var/lib/suricata/rules/local.rules

Add custom rule file in Suricata.
Defense OWASP10 attack's.

# Directory Structure
/opt/deception/
├── controller.py
└── risky_ips.json

/etc/nginx/conf.d/
├── deception_proxy.conf
└── risky_ips.map

/etc/systemd/system/
├── deception-controller.service
└── deception-controller.timer

/var/log/
├── nginx/
│   ├── deception_access.log
│   └── deception_error.log
└── suricata/
    ├── eve.json
    ├── fast.log
    └── suricata.log

# Command line's for Suricata.
* ip addr
* ip -br addr
* ip addr show enp2s0
* ip link show enp2s0
* suricata -V

* sudo suricata -T -c /etc/suricata/suricata.yaml
* sudo cat /var/lib/suricata/rules/local.rules

* sudo systemctl start suricata

* sudo tail -f /var/log/suricata/fast.log
* sudo tail -n 30 /var/log/suricata/fast.log
* sudo grep "192.168.85.144" \
  /var/log/suricata/fast.log
* sudo tail -f /var/log/suricata/eve.json
* sudo tail -f /var/log/suricata/stats.log

# Command line's for Nginx Reverse Proxy
* vi /etc/nginx/conf.d/deception_proxy.conf
* vi /etc/nginx/conf.d/risky_ips.map
* sudo nginx -t
* sudo systemctl start nginx

* sudo tail -f /var/log/nginx/deception_access.log
* sudo tail -f /var/log/nginx/deception_error.log

# Command line's for Risk Score Controller
* /opt/deception/controller.py
* sudo vi /opt/deception/controller.py
* /opt/deception/risky_ips.json // Risk data file

Run Controller manually
* sudo /opt/deception/controller.py
Run Controller Automation
* /etc/systemd/system/deception-controller.service
* vi /etc/systemd/system/deception-controller.service
* vi /etc/systemd/system/deception-controller.timer
enable automation
* sudo systemctl enable --now deception-controller.timer
