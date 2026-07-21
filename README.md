# Active-Deception
Making Active Decption system

Purpose of this system is trap attacker in honeypot(Fake Web) analize new attack pattern.

# BluePrint
VM 1 : Nginx, Suricata, Controller / IP : 192.168.85.100 / spec : 2 vCPU, 4GB 
VM 2 : Real Web / IP : 192.168.85.150 / spec : 2 vCPU, 4GB 
VM 3 : HoneyPot Web  / IP : 192.168.85.160 / spec : 2 vCPU, 4GB 
VM 4 : Elasticsearch, Kibana / IP : 192.168.85.105 / spec : 4 vCPU, 8GB 

