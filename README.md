# Unmaintained Project

This project has been superseeded both in featureset and preservation of privacy by
the [dnsquery](https://cookie.engineer/projects/cyber-security/dnsquery.html) project
which features a DNS client, a DNS server, and a DNS SOCKS proxy whilst using a ronin
mechanism to resolve questions via upstream DNS-over-HTTPS or DNS-over-TLS queries.

## DNS SOCKS Proxy (dns-proxy)

This is a downstream fork of [jtripper/dns-tcp-socks-proxy](https://github.com/jtripper/dns-tcp-socks-proxy)
and tries to keep this project maintained and the [package on AUR](https://aur.archlinux.org/packages/dns-proxy-git)
up-to-date.


This simple DNS Socks Proxy allows to tunnel DNS requests via a SOCKS proxy
(e.g. a local TOR proxy daemon).

It will choose a random DNS server for each request from the file `resolv.conf`
when it contains more than one DNS server.

Example `resolv.conf`:

```
# Cloudflare Public DNS
nameserver 1.0.0.1
nameserver 1.1.1.1

# Google Public DNS
nameserver 8.8.8.8
nameserver 8.8.4.4

# IPv6 currently unsupported :(
# nameserver 2001:4860:4860::8888
# nameserver 2001:4860:4860::8844
```


## Usage

The daemon must be run as `root` in order to bind to port `53`.

```
Usage: ./dns-proxy [options]

With no parameters, the configuration file is read from 'dns-proxy.conf'.

* -n          -- No configuration file (socks: 127.0.0.1:9050, listener: 0.0.0.0:53).
* -h          -- Print this message and exit.
* config_file -- Read from specified configuration file.

Examples:

 dns-proxy -n;
 dns-proxy /path/to/dns-proxy.conf;
```


## Configuration

The configuration file format may contain any of the below lines, whereas the defaults
are incrementally overridden with the values given in the custom `dns-proxy.conf` file
(which is given via the `dns-proxy <path/to/dns-proxy.conf>` usage).

Lines that start with `#` are ignored and not interpreted, so you can use them to
comment out settings.

```
#                    -- comment
socks_addr           -- socks listener address
socks_port           -- socks listener port
listen_addr          -- address for the dns proxy to listen on
listen_port          -- port for the dns proxy to listen on (most cases 53)
set_user             -- username to drop to after binding
set_group            -- group to drop to after binding
resolv_conf          -- location of resolv.conf file to read from
rewrite_resolve_conf -- toggles whether or not to rewrite resolv.conf
log_file             -- location to log to (should be /dev/null unless debugging).
```

The `dns-proxy.conf` defaults:

```
# example comment
socks_addr          = 127.0.0.1
socks_port          = 9050
listen_addr         = 0.0.0.0
listen_port         = 53
set_user            = nobody
set_group           = nobody
resolv_conf         = resolv.conf
rewrite_resolv_conf = false
log_file            = /dev/null
```


## Installation

On Arch linux, the dns proxy can be installed from the AUR at: https://aur.archlinux.org/packages/dns-proxy-git.

```
git clone https://aur.archlinux.org/packages/dns-proxy-git;

cd dns-proxy-git;

cd ./package/arch-linux;
makepkg -s;
sudo pacman -U dns-proxy-*.pkg.tar.xz;


# edit /etc/dns-proxy/dns-proxy.conf as necessary
sudo systemctl start dns-proxy

# enable autostart on bootup
sudo systemctl enable dns-proxy
```

On other distributions:

```
git clone https://github.com/cookiengineer/dns-proxy.git;

cd dns-proxy;

make;

# edit dns-proxy.conf as necessary
./dns-proxy
```


After running the daemon, the system should be configured to use the proxy automatically
if the `rewrite_resolv_conf` flag was set to `true`.


## License

The DNS TCP Socks Proxy is licensed under [GPL2](./LICENSE).


## Credits

- jtRIPper 2012 (creator)
- @ehrhardt (bugfix contributor)
- @lilydjwg (bugfix contributor)
- Brock Noland (bugfix contributor)
- @thuovila (bugfix contributor)
- @cookiengineer 2018+ (maintainer)

