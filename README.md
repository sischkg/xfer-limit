# Xfer-Limit Patches

These patches provide the limitation of AXFR data size for "Bind", "NSD", "knotDNS".

## Usage

### Bind

This patch for Bind-9.10.3.

    $ wget https://raw.githubusercontent.com/sischkg/xfer-limit/master/bind-9.10.3-xfer-limit-0.0.1.patch
    $ wget http://ftp.isc.org/isc/bind9/9.10.3/bind-9.10.3.tar.gz
    $ tar xzf bind-9.10.3.tar.gz
    $ cd bind-9.10.3
    $ patch -p1 < ../bind-9.10.3-xfer-limit-0.0.1.patch
    $ ./configure <configure options>
    $ make
    $ su
    # make install

Add `max-transfer-size-in` to zone statement in named.conf.
`max-transfer-size-in` specifies the upper size limit(bytes) of received AXFR data.

    zone "example.com" {
        type slave;
        masters { 192.168.198.132; };
        file "example.com.db";
        max-transfer-size-in 2000000;
    };


### NSD

    $ wget https://raw.githubusercontent.com/sischkg/xfer-limit/master/nsd-4.1.5-xfer-limit-0.0.1.patch
    $ wget http://www.nlnetlabs.nl/downloads/nsd/nsd-4.1.5.tar.gz
    $ tar xzf nsd-4.1.5.tar.gz
    $ cd nsd-4.1.5
    $ patch -p1 < ../nsd-4.1.5-xfer-limit-0.0.1.patch
    $ ./configure <configure options>
    $ make
    $ su
    # make install

Add `size-limit-xfr` to the zone attribute in nsd.conf.
`size-limit-xfr` specifies the upper size limit(bytes) of AXFR temporary file.

    name: "example.com"
        zonefile: "example.com"
        request-xfr: 192.168.198.132 NOKEY
        size-limit-xfr: 2000000

### knotDNS

    $ wget https://raw.githubusercontent.com/sischkg/xfer-limit/master/knot-1.6.5-xfer-limit-0.0.1.patch
    $ wget https://secure.nic.cz/files/knot-dns/knot-1.6.5.tar.xz
    $ xz -cd knot-1.6.5.tar.gz | tar xf -
    $ cd knot-1.6.5
    $ patch -p1 < ../knot-1.6.5-xfer-limit-0.0.1.patch
    $ ./configure <configure options>
    $ make
    $ su
    # make install

Add `xfr-in-limit` to the zone options in knot.conf.
`xfr-in-limit` specifies the upper size limit(bytes) of received AXFR data.

    example.com {
      file "example.com.zone";
      xfr-in master0;
      xfr-in-limit 2000000;
      notify-in master0;
    }

