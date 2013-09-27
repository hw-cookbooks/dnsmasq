# DNSMasq

Install and configure dnsmasq. Depends on the [hosts_file cookbook](https://github.com/hw-cookbooks/hosts_file)

# Recipes

## default
Installs the `dnsmasq` package. Depending on the `[:dnsmasq][:enable_dns]` and `[:dnsmasq][:enable_dhcp]` attributes it includes the `dns` and `dhcp` recipes respectively.

## dhcp

Includes the `default` recipe and writes the contents of the `node[:dnsmasq][:dhcp]` attribute hash to `/etc/dnsmasq.d/dhcp.conf`.

## dns

Includes the `default` and `manage_hostsfile` recipes, then writes the content of the `node[:dnsmasq][:dns]` attribute hash to `/etc/dnsmasq.d/dns.conf`.

## manage_hostsfile

Loads the `dnsmasq` data bag `managed_hosts` item and merges it with any nodes in the `[:dnsmasq][:managed_hosts]` attribute, then writes them out the the `/etc/hosts/` via the `hosts_file` cookbook.

# Usage

## Data Bag

If you need manage your DNS hosts you may use the `dnsmasq` data bag `managed_hosts` item. It takes the form:

```json
{
    "id": "managed_hosts",
    "192.168.0.100": "www.google.com",
    "192.168.0.101": ["www.yahoo.com", "www.altavista.com"]
}
```

## Attributes

* `[:dnsmasq][:enable_dns]` whether to enable the DNS service, default is `true`
* `[:dnsmasq][:enable_dhcp]` whether to enabled the DHCP service, default is `false`
* `[:dnsmasq][:managed_hosts]` hash of IPs and hostname/array of hostnames for the `manage_hostfile` recipe, default is empty
* `[:dnsmasq][:managed_hosts_bag]` name of the data bag item, default is `managed_hosts`
* `[:dnsmasq][:dhcp]` = hash of settings and values for the `/etc/dnsmasq.d/dhcp.conf`, default is empty
* `[:dnsmasq][:dns]` hash of settings and values for the `/etc/dnsmasq.d/dns.conf`, defaults are

```ruby
{
  'no-poll' => nil,
  'no-resolv' => nil,
  'server' => '127.0.0.1'
}
```

# Repo:

* https://github.com/hw-cookbooks/dnsmasq
