# Ansible Role for Bind Nameserver

## Variables

- `bind_group`: Optional. Name of ansible group for all members participating.
- `bind_primary`: If `bind_group` is set, required to be set on exactly 1 node
  that will act as primary.
- `bind_ip`. If `bind_group` is set, required for the ip address for things
  like secondary update notifications.
- `bind_recursive`: Boolean.  Defaults to true.  Whether or not to allow recursive
  DNS queries.
- `bind_use_ipv4`: Boolean. Defaults to false.  Set this to force bind to use
  ipv4 for recursive queries.  May be need to prevent timeouts if ipv6 is not
  enabled in the environment.
- `bind_forward`: List of domains to forward to a different resolver.  Each
  one contains these attributes:
  - `domain`: Domain to forward
  - `servers`: List of server ip addresses to forward to.
- `bind_keys`: List of bind keys configured.  They must be generated via
  `tsig-keygen`
  - `name`: Name to assign to key, must be unique.
  - `algorithm`: Algorithm associated with key.  Defaults to `hmac-sha256` if
    not already specified.
  - `secret`: Base64 encoded secet.
- `bind_zones`: List of BIND zones this server is authoritative for.  Each list
  contains these attributes.
  - `dynamic_key`: The key name for a dynamic key for updates.  If specified
    this zone becomes a dynamic zone and one server will become primary and
    the remaining will be secondaries and replicate from primary.
  - `domain`: Domain for Zone.
  - `records`: List of records.
    - `name`: Required. Name of entry.  This should be the prefix and not
      include the domain of the zone.  Use `@` to apply to base domain.  Can
      also use `*` for wildcard.
    - `addresses`: List of IPv4 and IPv6 addresses associated with the host.
      This will automatically emit `A` or `AAAA` records as required without
      needing to specify a record type.
    - `ttl`: TTL for the record.  Defaults to `600` if not specified.
    - `type`: DNS record type. Used for records other than `A` or `AAAA`.
      `CNAME`, `TXT`, `SRV`, and `URI` are currently supported.
    - `data`: Data associated with `CNAME`, `TXT`, or `SRV` records.  For a
      `CNAME` or `SRV` record, this is the FQDN pointed to, for `TXT` this is
      the text data, for `URI` records this is the properly formatted uri
      target, e.g. `krb5srv:m:tcp:ipa1.ipa.pc.testenv.bradhouse.dev.`
    - `priority`: `SRV` and `URI` record priority. Defaults to `0`.
    - `weight`: `SRV` and `URI` record weight. Defaults to `100`.
    - `port`: `SRV` record port. Required.

