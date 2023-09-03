# M239

> Gemäss AB204

## BIND9 - Konfiguration

- Zunächst werden die nötigen Konfigurationsdateien angelegt:

```bash
sudo touch named.conf
sudo touch named.conf.local
sudo touch named.conf.options
```

- Anschliessend wird folgender Inhalt in `named.conf` eingefügt:

```bash
include "/etc/bind/named.conf.options";
include "/etc/bind/named.conf.local";

options {

};
```

- Foldender Inhalt ins `named.conf.options`:

```bash
options {
    
    // IPV4
    listen-on port 53 {
        any;
    };

    // IPV6
    listen-on-v6 {
        none;
    };

    // DNS-Anfragen an Quad9 weiterleiten
    forwarders {
        1.1.1.1;
        9.9.9.9;
    };

    // Anfragen von uerberall moeglich
    allow-query {
        any;
    };

    // Rekusrive Anfragen fuer Clients
    allow-recursion {
        any;
    };
};
```

- Server neustarten

```bash
sudo reboot now
```

- `BIND9` starten, mit Logs

```bash
sudo named -g
```

![Alt text](image-9.png)

- `dig` erneut versuchen

```bash
dig www.google.ch @127.0.0.1
```

![Alt text](image-10.png)

### Konfigurationen vmLS1, vmWP1, vmLP1

Die Konfigurationen von vmWP1 und vmLP1 werden übers GUI gemacht - vmLS1 wird übers CLI bearbeitet:

vmWP1: ![Alt text](image-11.png)

vmLP1: ![Alt text](image-12.png)

vmLS1: ![Alt text](image-13.png)

Die Änderungen in vmLS1 müssen noch mit `sudo netplan apply` aktiviert werden.

## Fehlersuche

Fehler in der Konfiguraton würden im Bind log erscheinen. Dieser findet sich unter `/var/log/syslog`.

## Kommentare

Um Kommentare in einem `.yaml` File zu schreiben, benötigen diese ein `//` davor.

## Einrücken

Die Einrückung ist bei der Arbeit mit `.yaml` Files sehr ausschlaggebend, und oft auch der Grund für viele Fehlkonfigurationen.

Wichtig ist hier, dass man durchgehend entweder mit Tabulator oder mit Leerschlägen einrückt, und diese Methoden nicht durchmischt anwendet.

## Liste der Root-Server aktualisieren

Um diese Liste zu aktualisieren, kann mal als `su` folgenden Befehl ausführen:

```bash
dig @a.root-servers.net | grep -E -v ';|^$' sort > /etc/bind/db.root
```

## DNS-Zonen einrichten

### Netzwerkübersicht

![Alt text](image-14.png)

### Zonendatei für die DMZ

```
;
; Zonendatei für dmz.mattefit.ch.
; /etc/bind/db.ch.mattefit.dmz
;
$TTL    3600
@       IN      SOA     vmls1.dmz.mattefit.ch.      root.mattefit.ch. (
                         1
                        1H
                        2H
                        1D
                        1H )

@       IN      NS      vmls1.dmz.mattefit.ch.
vmlf1   IN      A       192.168.220.1
vmls1   IN      A       192.168.220.10
```

```
;
; Zonendatei für 220.168.192.in-addr.arpa.
; /etc/bind/db.192.168.220
;
$TTL    3600
@       IN      SOA     vmls1.dmz.mattefit.ch.      root.mattefit.ch. (
                         1
                        1H
                        2H
                        1D
                        1H )

@       IN      NS      vmls1.dmz.mattefit.ch.
1       IN      PTR     vmlf1.dmz.mattefit.ch.
10      IN      PTR     vmls1.dmz.mattefit.ch.
```

```
//
// DMZ
//
zone "dmz.mattefit.ch" {
        type master;
        notify no;
        file "/etc/bind/db.ch.mattefit.dmz";
};
zone "220.168.192.in-addr.arpa" {
        type master;
        notify no;
        file "/etc/bind/db.192.168.220";
};
```

```
;
; Zonendatei für lan.mattefit.ch.
; /etc/bind/db.ch.mattefit.lan
;
$TTL    3600
@       IN      SOA     vmwp1.lan.mattefit.ch.      root.mattefit.ch. (
                         1
                        1H
                        2H
                        1D
                        1H )

@       IN      NS      vmwp1.lan.mattefit.ch.
vmlf1   IN      A       192.168.110.1
vmwp1   IN      A       192.168.110.10
```

```
;
; Zonendatei für 110.168.192.in-addr.arpa.
; /etc/bind/db.192.168.110
;
$TTL    3600
@       IN      SOA     vmwp1.lan.mattefit.ch.      root.mattefit.ch. (
                         1
                        1H
                        2H
                        1D
                        1H )

@       IN      NS      vmwp1.lan.mattefit.ch.
1       IN      PTR     vmlf1.lan.mattefit.ch.
10      IN      PTR     vmwp1.lan.mattefit.ch.
```

```
//
// LAN
//
zone "lan.mattefit.ch" {
        type master;
        notify no;
        file "/etc/bind/db.ch.mattefit.lan";
};
zone "110.168.192.in-addr.arpa" {
        type master;
        notify no;
        file "/etc/bind/db.192.168.110";
};
```