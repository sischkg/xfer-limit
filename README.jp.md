# Xfer-Limit Patches

このパッチは、BINDやNSD、knotDNS、PowerDNSにゾーン転送(AXFR)で受信するデータサイズを制限する機能を追加します。

## 使用方法

### BIND

BIND 9.10.3に対するパッチです。パッチは以下の手順で当てることができます。

    $ wget https://raw.githubusercontent.com/sischkg/xfer-limit/master/bind-9.10.3-xfer-limit-0.0.1.patch
    $ wget http://ftp.isc.org/isc/bind9/9.10.3/bind-9.10.3.tar.gz
    $ tar xzf bind-9.10.3.tar.gz
    $ cd bind-9.10.3
    $ patch -p1 < ../bind-9.10.3-xfer-limit-0.0.1.patch
    $ ./configure <configure options>
    $ make
    $ su
    # make install

BIND 9.9.1-P1に対するパッチです。

    $ wget https://raw.githubusercontent.com/sischkg/xfer-limit/master/bind-9.9.9-P1-xfer-limit-0.0.1.patch
    $ wget ftp://ftp.isc.org/isc/bind9/9.9.9-P1/bind-9.9.9-P1.tar.gz
    $ tar xzf bind-9.9.9-P1.tar.gz
    $ cd bind-9.9.9-P1
    $ patch -p1 < ../bind-9.9.9-P1-xfer-limit-0.0.1.patch
    $ ./configure <configure options>
    $ make
    $ su
    # make install

named.confに`max-transfer-size-in`を設定することで、
ゾーン転送の受信データの上限値(bytes)を設定することができます。

    zone "example.com" {
        type slave;
        masters { 192.168.198.132; };
        file "example.com.db";
        max-transfer-size-in 2000000;
    };

### NSD

NSD 4.1.10に対するパッチです。パッチは以下の手順で当てることができます。

    $ wget https://raw.githubusercontent.com/sischkg/xfer-limit/master/nsd-4.1.10-xfer-limit-0.0.2.patch
    $ wget http://www.nlnetlabs.nl/downloads/nsd/nsd-4.1.10.tar.gz
    $ tar xzf nsd-4.1.10.tar.gz
    $ cd nsd-4.1.10
    $ patch -p1 < ../nsd-4.1.10-xfer-limit-0.0.2.patch
    $ ./configure <configure options>
    $ make
    $ su
    # make install

nsd.confに`size-limit-xfr`を設定することで、
ゾーン転送の受信データを保存する一時ファイルの上限値(bytes)を設定することができます。

    zone:
        name: "example.com"
        zonefile: "example.com"
        request-xfr: 192.168.198.132 NOKEY
        size-limit-xfr: 2000000

### knot DNS

knot DNS 1.6.5に対するパッチです。パッチは以下の手順で当てることができます。

    $ wget https://raw.githubusercontent.com/sischkg/xfer-limit/master/knot-1.6.5-xfer-limit-0.0.1.patch
    $ wget https://secure.nic.cz/files/knot-dns/knot-1.6.5.tar.xz
    $ xz -cd knot-1.6.5.tar.gz | tar xf -
    $ cd knot-1.6.5
    $ patch -p1 < ../knot-1.6.5-xfer-limit-0.0.1.patch
    $ ./configure <configure options>
    $ make
    $ su
    # make install

knot.confに`xfr-in-limit`を設定することで、
ゾーン転送の受信データの上限値(bytes)を設定することができます。

    example.com {
      file "example.com.zone";
      xfr-in master0;
      xfr-in-limit 2000000;
      notify-in master0;
    }


### PowerDNS

PowerDNS 3.4.7に対するパッチです。パッチは以下の手順で当てることができます。

    $ wget https://raw.githubusercontent.com/sischkg/xfer-limit/master/powerdns-3.4.7-xfer-limit-0.0.1.patch
    $ wget https://downloads.powerdns.com/releases/pdns-3.4.7.tar.bz2
    $ tar xjf pdns-3.4.7.tar.bz2
    $ cd pdns-3.4.7
    $ patch -p1 < ../powerdns-3.4.7-xfer-limit-0.0.1.patch
    $ ./configure <configure options>
    $ make
    $ su
    # make install

domainmetadataテーブルにXFR-SIZE-LIMITを追加することで
ゾーン転送の受信データの上限値(bytes)を設定することができます。

    mysql> select id from domains where name = 'example.com';
    +----+
    | id |
    +----+
    |  2 |
    +----+
    1 row in set (0.00 sec)
    
    mysql> insert into domainmetadata (domain_id, kind, content) values (2,'XFR-SIZE-LIMIT','10000000');

    mysql> select * from domainmetadata;
    +----+-----------+----------------+----------+
    | id | domain_id | kind           | content  |
    +----+-----------+----------------+----------+
    |  2 |         2 | XFR-SIZE-LIMIT | 10000000 |
    +----+-----------+----------------+----------+
    1 row in set (0.00 sec)

