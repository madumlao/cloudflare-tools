# cloudflare-tools
Command line tools for working with cloudflare DNS API. Requires only bash, curl, and jq.

To install, copy or link all of the commands into your PATH.

## Examples
~~~bash
# list all cloudflare zones
cloudflare-getzones -t $CLOUDFLARE_TOKEN
[
  {
    "id": "ZONE-ID-1",
    "name": "madumlao.com"
  }
]
~~~

~~~bash
# list all cloudflare zones, with CLOUDFLARE_TOKEN as environment variable
export CLOUDFLARE_TOKEN=MYTOKENHERE
cloudflare-getzones
[
  {
    "id": "ZONE-ID-1",
    "name": "madumlao.com"
  }
]
~~~

The succeeding exmaples all assume the `CLOUDFLARE_TOKEN` environment variable is populated.

~~~bash
# search for a specific cloudflare zone by name or ID
cloudflare-searchzones madumlao.com
[
  {
    "id": "ZONE-ID-1",
    "name": "madumlao.com"
  }
]
~~~

~~~bash
# list all DNS entries for a zone
cloudflare-getdns madumlao.com
[
  {
    "id": "RECORD-ID-1",
    "type": "A",
    "name": "madumlao.com",
    "content": "1.2.3.1",
    "proxied": false,
    "ttl": 1,
    "priority": null
  },
  {
    "id": "RECORD-ID-2",
    "type": "MX",
    "name": "mail.madumlao.com",
    "content": "10.20.30.40",
    "proxied": false,
    "ttl": 1,
    "priority": 10
  },
  {
    "id": "RECORD-ID-3",
    "type": "TXT",
    "name": "madumlao.com",
    "content": "my text content here",
    "proxied": false
    "ttl": 1,
    "priority": null
  },
  {
    "id": "RECORD-ID-4",
    "type": "A",
    "name": "server.madumlao.com",
    "content": "1.2.3.2",
    "proxied": false
    "ttl": 1,
    "priority": null
  }
]
~~~

~~~bash
# search for a specific DNS record
cloudflare-searchdns madumlao.com server1.madumlao.com
[
  {
    "id": "RECORD-ID-4",
    "type": "A",
    "name": "server.madumlao.com",
    "content": "1.2.3.2",
    "proxied": false
    "ttl": 1,
    "priority": null
  }
]

cloudflare-searchdns madumlao.com madumlao.com
[
  {
    "id": "RECORD-ID-1",
    "type": "A",
    "name": "madumlao.com",
    "content": "1.2.3.1",
    "proxied": false,
    "ttl": 1,
    "priority": null
  },
  {
    "id": "RECORD-ID-3",
    "type": "TXT",
    "name": "madumlao.com",
    "content": "my text content here",
    "proxied": false
    "ttl": 1,
    "priority": null
  }
]
~~~

~~~bash
# filter dns records by type
cloudflare-searchdns -T TXT madumlao.com madumlao.com
[
  {
    "id": "RECORD-ID-3",
    "type": "TXT",
    "name": "madumlao.com",
    "content": "my text content here",
    "proxied": false
    "ttl": 1,
    "priority": null
  }
]
~~~

~~~bash
# update a DNS record with a static IP
cloudflare-patchdns -i 1.2.3.4 madumlao.com server1.madumlao.com
{
  "id": "RECORD-ID-4",
  "zone_id": "ZONE-ID-1",
  "zone_name": "madumlao.com",
  "type": "A",
  "content": "1.2.3.4",
  "proxiable": true,
  "proxied": false,
  "ttl": 1,
  "locked": false,
}
~~~

~~~bash
# update a DNS record by determining current public IP
cloudflare-patchdns madumlao.com server1.madumlao.com
{
  "id": "RECORD-ID-4",
  "zone_id": "ZONE-ID-1",
  "zone_name": "madumlao.com",
  "type": "A",
  "content": "20.30.40.50",
  "proxiable": true,
  "proxied": false,
  "ttl": 1,
  "locked": false,
}
~~~

---

## Common options
~~~bash
-h|--help        : this help
-u|--url URL     : cloudflare api url (defaults to `$CLOUDFLARE_URL` or https://api.cloudflare.com/client/v4)
-t|--token TOKEN : cloudflare token   (defaults to `$CLOUDFLARE_TOKEN`)
~~~
