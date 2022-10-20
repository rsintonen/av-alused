1. From Ping to HTTP

NB! Enne tuleks teha need Udacity kursused:
linux command line basics
configuring linux web servers
designing restful apis

Setupi variandid:
Hosted või Local VM option

Käivita google cloud shell (hosted virtuaalne server) console.cloud.google.com
Mis cloud shelle veel on? Amazon AWS?

Step by step:
1. Installing networking tools
sudo apt-get update && sudo apt-get upgrade
	sudo - superuser do (adminiõigused asjade installimiseks)
	&& - kui esimene käsk õnnestub, käivita teine
Teeb VMis vajalikud uuendused

sudo apt-get install netcat-openbsd tcpdump traceroute mtr
Installib "network utility programmid" - mis see tähendab, mis seal sisaldub?

Küsimus: "Which MySQL product do you want to configure?"

ip COMMAND CHEAT SHEET (koos käskude kirjeldustega): https://access.redhat.com/sites/default/files/attachments/rh_ip_command_cheatsheet_1214_jcs_print.pdf

Abi saamiseks kirjuta tühi käsk (nt 'host'), kuvab sellega seotud lisavalikud jne

ip addr show eth0 - ip 'käskude seeria', addr show 'näita aadressi', eth0 '0 (esimene) Ethernet port'

ip route show

ping -c3 8.8.8.8 - 'Ping sends an ICMP ECHO_REQUEST to a host in order to test if the host is reachable via the network.' - '-bash: ping: command not found', installin sudo apt-get install iputils-ping.

host -t aaaa google.com - aaaa 'AAAA-record is useds to specify the IPv6 address for a host'

host -t mx udacity.com - mx 'An MX host is a mail server (SMTP server), a computer permanently connected to the
internet with a fixed IP address configured to manage e-mail for one (or more) domain names'

tcpdump -n -c5 -i eth0 port 22 - installisin tcpdumpi (sudo apt-get install tcpdump), vastus 'You don't have permission to capture on that device'

traceroute www.udacity.com - 'Prints the route that a packet takes to reach the host. This command is useful when you want to know about the route and about all the hops that a packet takes.'
'(172.64.146.67), 30 hops max, 60 byte packets'


mtr www.udacity.com - 'MTR is a network diagnostic tool that combines ping and traceroute into one program (it refreshes on its own and gives you a live look at network response and connectivity).'

printf 'HEAD / HTTP/1.1\r\nHost: www.udacity.com\r\n\r\n' \ | nc www.udacity.com 80 - 
	printf on mingi printout command
	nc - netcat 'is a computer networking utility for reading from and writing to network connections using TCP or UDP.'
	| võtab esimese käsu (printf jnejne) ja saadab selle nc abil aadressile www.udacity.com port 80, ja trükib vastuse

9. printf and netcat illustrate layers
	Layer models:
		Application (HTTP, SSH) - URL, passwords, web pages
		Transport (TCP, UDP) - port numbers
		Internet (IP) - IP addresses, routes
		Hardware (Wifi, ethernet, DSL) - signal strength, access points

man nc - trükib konkreetse käsu kasutusjuhendi

10. Listen on a Port
	nc -l 3456 - kuulab kohalikku porti 3456. kui connectida teises kliendis nc localhost 3456, siis saab andmeid vahetada


2. Names and Addresses

packetil on destination IP

host - machine on the internet
endpoint - two machines or programs communicating over a connection

trükin hostname'i sisse, dns (domain name system) konvertib selle ip-ks
DNS sisaldab igasugu kirjeid, nt A-kirje (IPv4) või AAAA-kirje (IPv6)

CNAME - 'Canonical name', näitab, kas antud hostname on mõne teise hostname'i alias
AAAA - hostname'i IPv6 aadress
A - hostname'i IPv4 aadress
NS - 'Name server record', viitab, milistes DNS nimeserverites antud hostname'i kohta kirjed eksisteerivad

Kuidas on nimeserverid grupeeritud? COM, EE, vms?
cache TTL - time to live (kaua andmed cache serveris istuvad, enne kui kustutatakse)

Kaks põhilist aadressitüüpi: IPv4 ja IPv6
8 oktetti xxx.xxx.xxx.xxx