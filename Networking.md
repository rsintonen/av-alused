### 1. From Ping to HTTP

NB! Enne tuleks teha need Udacity kursused:
linux command line basics ( https://www.udacity.com/course/linux-command-line-basics--ud595 )
configuring linux web servers ( https://www.udacity.com/course/configuring-linux-web-servers--ud299 )
designing restful apis ( https://www.udacity.com/course/designing-restful-apis--ud388 )

Setupi variandid:
Hosted või Local VM option

Käivita google cloud shell (hosted virtuaalne server) console.cloud.google.com
Mis cloud shelle veel on? Amazon AWS?

Step by step:
1. Installing networking tools

`sudo apt-get update && sudo apt-get upgrade`

sudo - superuser do (adminiõigused asjade installimiseks)
&& - kui esimene käsk õnnestub, käivita teine

Teeb VMis vajalikud uuendused

`apt-get update`

uuendab paketinimekirjad
`upgrade #`

tarkvarauuendus, pole mõtet teha (võib vahele jätta)

võib asendada:

`sudo apt-get install netcat-openbsd tcpdump traceroute mtr iputils-ping lsof`

Installib "network utility programmid" - mis see tähendab, mis seal sisaldub?

Küsimus: "Which MySQL product do you want to configure?" < get-apt upgrade'i hulka kuulub, pole vaja teha

ip COMMAND CHEAT SHEET (koos käskude kirjeldustega): https://access.redhat.com/sites/default/files/attachments/rh_ip_command_cheatsheet_1214_jcs_print.pdf

* Abi saamiseks kirjuta tühi käsk (nt 'host'), kuvab sellega seotud lisavalikud jne

`ip addr show eth0` - ip 'käskude seeria', addr show 'näita aadressi', eth0 '0 (esimene) Ethernet port'

`ip route show`

`ping -c3 8.8.8.8` - 'Ping sends an ICMP ECHO_REQUEST to a host in order to test if the host is reachable via the network.' - '-bash: ping: command not found', installin sudo apt-get install iputils-ping.

`host -t aaaa google.com` - aaaa 'AAAA-record is useds to specify the IPv6 address for a host'

`host -t mx udacity.com` - mx 'An MX host is a mail server (SMTP server), a computer permanently connected to the internet with a fixed IP address configured to manage e-mail for one (or more) domain names'

`tcpdump -n -c5 -i eth0 port 22` - installisin tcpdumpi (sudo apt-get install tcpdump), vastus 'You don't have permission to capture on that device'

`traceroute www.udacity.com` - 'Prints the route that a packet takes to reach the host. This command is useful when you want to know about the route and about all the hops that a packet takes.' (vt edaspidi TCP teemal)

'(172.64.146.67), 30 hops max, 60 byte packets'

`mtr www.udacity.com` - 'MTR is a network diagnostic tool that combines ping and traceroute into one program (it refreshes on its own and gives you a live look at network response and connectivity).'

`printf 'HEAD / HTTP/1.1\r\nHost: www.udacity.com\r\n\r\n' \ | nc www.udacity.com 80` - 

printf on mingi printout command

nc - netcat 'is a computer networking utility for reading from and writing to network connections using TCP or UDP.'

| võtab esimese käsu (printf jnejne) ja saadab selle nc abil aadressile www.udacity.com port 80, ja trükib vastuse

9. printf and netcat illustrate layers
Layer models:
1. Application (HTTP, SSH) - URL, passwords, web pages
2. Transport (TCP, UDP) - port numbers
3. Internet (IP) - IP addresses, routes
4. Hardware (Wifi, ethernet, DSL) - signal strength, access points

`man nc` - trükib konkreetse käsu kasutusjuhendi

10. Listen on a Port
	`nc -l 3456` - kuulab kohalikku porti 3456. kui connectida teises kliendis nc localhost 3456, siis saab andmeid vahetada
---
### 2. Names and Addresses

* Packetil on kindel addressaat (IP)
* host - mingi võrgus olev seade
* endpoint - seadmed või programmid, mis omavahel üle võrguühenduse suhtlevad.

Trükin hostname'i sisse, DNS (domain name system) konvertib selle IP-ks.

DNS sisaldab igasugu kirjeid, nt A-kirje (IPv4) või AAAA-kirje (IPv6).

CNAME - 'Canonical name', näitab, kas antud hostname on mõne teise hostname'i alias

AAAA - hostname'i IPv6 aadress

A - hostname'i IPv4 aadress

NS - 'Name server record', viitab, milistes DNS nimeserverites antud hostname'i kohta kirjed eksisteerivad

Kuidas on nimeserverid grupeeritud? COM, EE, vms?

Cache TTL - time to live (kaua andmed cache serveris istuvad, enne kui kustutatakse)

Kaks põhilist aadressitüüpi: IPv4 ja IPv6

8 oktetti xxx.xxx.xxx.xxx

---
### 3. Addressing and Networks

IPv4 32-bit / 4 octet (dotted quad)

Kõiki ei kasutata pärisaadresside jaoks: mõned on kasutusel eriprotokollide jaoks (special protocols), samuti testimiseks või dokumenteerimiseks.

Veidi üle 1/8 võimalikest IPv4 aadressidest on mõeldud muuks kui public host-ide adresseerimiseks.

IPv4 address map (vt 3. Reserved IP addresses):

Helerohelised plokid (0, 10 ja 127) on täielikult reserveeritud.

Tumerohelised on osaliselt reserveeritud (nt 192 plokk on osaliselt).

Tsüaanivärvi rivi (alates 224) on IP multicasti jaoks (teatud üks-mitu või mitu-mitu andmeedastuse tehnika).

Oranž rivi (alates 240) on ära blokeeritud (pole võimalik kasutada).

198.51.100.0/24 bit või 0/22 bit
Ühes võrgus olevatel seadmetel on sarnased IP-aadressid (jagavad esimest 22 või 24 bitti ning viimased 8 või 10 bitti on erinevad; 10-bitise aadressi puhul saab võrgus olla rohkem seadmeid (256 vs 1024).

Aadressiploki esimene ja viimane aadress on enamasti reserveeritud (esimene aadress on ruuteri oma).

Misasi on subnet mask? Lihtsalt mingi aadress standardkujul (255.255.255.0 või 255.255.0.0)?

Subnet maski arvutamiseks jaota aadress oktettideks ja vaata binaarväärtust; fikseeritud aadress on 1 ja vaba aadress on 0.

0/14 = 255.252.0.0 (11111111.11111100.00000000.00000000)

Igal node'il on oma aadress, kuhu pakette saadetakse.

IP'd ei kuulu hostidele, vaid interface'idele, mis hostide peal jooksevad. Hostil võib olla mitu interface'i, millel omakorda üks või rohkem aadresse.

Nt laptopil võib olla ethernet, wifi interface või loopback interface.

Loopback interface'i saab kasutada samal hostil olevate interface'ide vaheliseks suhtluseks.

Nt koduses ruuteris on üks interface võrku ühendumiseks, lisaks nt lan interface'id kaabli või wifiantenn wifiseadmete ühendamiseks.

Linuxi peal saab leida interface'id käsuga "ip addr show".

11. Routers and default gateways

Ruuter on gateway erinevate võrkude omavaheliseks ühendamiseks.

Samas võrgus olevatel hostidel on sama ploki ip-aadressid (ip addresses on the same net block).

* Mis on default gateway, kus seda kasutatakse ja kus mitte?

cmd-s käsuga `netstat -nr` saan kohaliku ip 192.168.0.1

käsuga `ping 192.168.0.1` saan keskmise response aja 40 ms

13. NAT

IPv4 aadresside vähesuse tõttu on kasutatud meetodit, kus kohalikule gatewayle (ruuterile) antakse üks IP-aadress ja kõik selle küljes olevad seadmed jagavad välist IP-d, aga võrgusiseselt on kõigil oma privaatne IP.

Privaatsete IP-de jaoks on 3 reserveeritud plokki:
10.0.0.0 / 8

172.16.0.0 / 12

192.168.0.0 / 16

kõige tavalisem on 192.168.0.0 / 24 (ja default gateway 192.168.0.1)

(Spetsifikatsioon selle jaoks on RFC1918)

Privaatseid IP-sid kasutatakse NAT-is (network address translation).

Hosti suhtlemisel välisvõrguga peab ruuter privaatse IP "tõlkima".

14. Private and public

Privaatne IP tähendab, et seda saab kasutada ainult sisevõrgu piires (kaob üheselt mõistetav tähendus väljaspool sisevõrku, kuna nad pole tegelikult unikaalsed).

cmd-s otsin `ipconfig` abil oma IPv4 (private); googeldan "what's my ip" ja saan avaliku IP.

15. IPv6
IPv4 aadresside puudujäägi lahendamiseks võeti kasutusele IPv6. Need aadressid on pikemad (128 bit / 16 oktetti), mistõttu kombinatsioone on rohkem. IPv6 kirjutatakse oktettide kaupa hexadecimal numbritega ja koolonitega eraldatult.

---
### 4. Protocol Layers

TCP sessioni abil saavad kaks programmi omavahel andmeid üle võrgu edastada.

TCP ise asub ühel 'protocol stack'i (protokollipinu) kihil.

Protokollipinu ülesehitus:

Riistvara > IP > TCP > HTTP

Iga pinu järgmine kiht sõltub omakorda eelmises kihis tehtud toimingutest.

Alumiste kihtide vead tulevad välja nt veateadete või aeglaste vastustena.

Veebirakendused kasutavad HTTP-kihti serveritesse päringute tegemiseks

TCP, UDP, ICMP > erinevad protokollid

TCP on operatsioonisüsteemi sisse ehitatud, brauserid kasutavad seda OS-i funktsionaalsust.

IP omakorda toetub seadmetele ja draiveritele, et andmeid liigutada.

`tcpdump` abil saab kuvada hostist väljuvat ja sinna sisenevat liiklust.

TCP kuulamiseks:

`sudo tcpdump -n host *IP*`

tcpdump näitab (nt pingimisel, dns requestidel, http requestidel) saadetud ja vastu võetud pakette.

tcpdump tuleb suunata kuulama vastavat hosti, porti vms.

HTTP requesti puhul näitab tcpdump, et enne andmete (payload) edastamist tuleb eelnevalt ühendus üles seada (umbes nagu call/response).

Toimingute järjekord:
1. Ühenduse loomine (Client teab host name'i ja porti, aga server kliendi kohta midagi ei tea. Client peab esmalt saatma oma IP ja pordi.)
2. Vastus serverilt (acknowledgement/ACK); kui vastust ei tule, siis vastaspool saadab packeti uuesti.
3. Request (nt GET/)
4. Vastus serverilt jne.

TCP nummerdab packetid, et säilitada andmete õige järjekord.

Iga saadud packeti kohta annab saaja kinnituse (ACK), et on packeti kätte saanud.

tcpdump data näide:

19:51:58.304117 IP 10.20.27.153.59328 > 93.184.216.34.80: Flags [S], seq 2574797435, win 26883, options [mss 8961,sackOK,TS val 689168793 ecr 0,nop,wscale 7], length 0

kus nurksulgudes on erinevad requestid või teated:

SYN (synchronize) [S] — This packet is opening a new TCP session and contains a new initial sequence number.
Esimene packet on alati [S] flagiga, et avada TCP kliendi ja serveri vahel.

FIN (finish) [F] — This packet is used to close a TCP session normally. The sender is saying that they are finished sending, but they can still receive data from the other endpoint.

[F] näitab, et osapool on lõpetanud andmete saatmise (nt request)

PSH (push) [P] — This packet is the end of a chunk of application data, such as an HTTP request.

[P] tähistab andmekogumi viimast osa (?)

RST (reset) [R] — This packet is a TCP error message; the sender has a problem and wants to reset (abandon) the session.

ACK (acknowledge) [.] — This packet acknowledges that its sender has received data from the other endpoint. Almost every packet except the first SYN will have the ACK flag set.

URG (urgent) [U] — This packet contains data that needs to be delivered to the application out-of-order. Not used in HTTP or most other current applications.

Flagid on nähtavad TCP puhul; näiteks pingides (ICMP protokoll) või DNS lookupi puhul (UDP) flage näha ei ole.

Why do packets drop?

TCP reguleerib kiirust vastavalt teise poole vastustele (kui ACK tuleb aeglaselt, siis jääb ka saatmine aeglasemaks) - TCP congestion control

Ka näiteks suletud pordi puhul saadab ruuter mitu korda, kuid järjest aeglasemalt, pakette välja, kuni lõpuks tuleb timeout (TCP-l on sisseehitatud taimerid).

---
### 5. Big Networks

Andmete liikumine ühest kohast teise võtab aega ning kasutajad vihkavad ootamist.

Ruuterite vaheline liikumine on "hüpe" (hop).

Andmed liiguvad võrgus läbi mitme seadme (ruuterid);

Kõigi hüpete teada saamiseks saab kasutada `traceroute` tööriista

`tracert *aadress*`

(või ka teine tööriist: `mtr *aadress*`)

Ping näitab round trip time'i, packeti saatmise ja vastuse saamise järel
rtt näitab lähedust (võrgus)

traceroute sai alguse ühest turvafunktsioonist (safety feature), mis aitab ära hoida lõputuid silmuseid (infinite loop).

Kui ttl (time to live) läbi saab, siis saadab ruuter teate tagasi hostile ja annab aadressi, kus packet suri.

traceroute saadab packeti ja annab ttl=0, 1, 2... jne, et saada teada, mitu ruuterit vahel on, enne kui packet lõppadressaadini jõuab.

Bandwidthi hindamise põhimõte aeglaseima lüli järgi:

min(slow,fast)=slow

Võrgu dimensioneerimiseks oluline: alates lõppkasutaja poolt > bandwidth kasvab nii, et iga võrgu osa oleks võimeline teenindama enda järel olevat osa.

Latencyt mõjutab eelkõige kaugus ja hüpete arv.

Middleboxes 1: firewalls & filtering

Tulemüürid filtreerivad võrku sisenevat ja väljuvat liiklust.

Tulemüüride kasutusalad:
1. Turvalisus (ainult ette nähtud ühendused pääsevad läbi)
2. Tsensuur (blokeeritakse teatud sisu)

Kui kasutaja ei pääse su teenusele ligi, mida testida:
1. Kas kasutaja saab sinu IP-d pingida?
2. Kas kasutaja pääseb samal serveril olevatele teistele domeenidele ligi?
3. Kas serveri nimi tuleb välja HOST või DIG abil?

* 10. What can a filter do? > mingi error quiziga, luges õige vastuse valeks

Middleboxes 2: proxies & nat

NAT-seadmed, nt koduruuterid, toimivad ka lihtsa tulemüürina.

Teenusepakkujatel on omakorda implementeeritud carrier-grade nat

NAT töötab tcp kihil (kirjutab packetid üle), proxy töötab HTTP kihil.
