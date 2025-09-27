# Ansible Role for Bind Nameserver

## Variables

- `bind_recursive`: Boolean.  Defaults to true.  Whether or not to allow recursive
  DNS queries.
- `bind_zones`: List of BIND zones this server is authoritative for.  Each list
  contains these attributes.
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
      `CNAME` and `TXT` are currently supported.
    - `data`: Data associated with `CNAME` or `TXT` records.  For a `CNAME` this
      is the FQDN pointed to, for `TXT` this is the text data.

