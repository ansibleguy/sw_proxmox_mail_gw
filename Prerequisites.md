# Prerequisites


## System requirements

Make sure your target system meets the [system requirements](https://pmg.proxmox.com/pmg-docs/pmg-admin-guide.html#system_requirements)!

  - The target system needs to have a public IP.
  - You need to have a public Domain.
  - ...

## Firewalling

See also: [Documentation](https://pmg.proxmox.com/pmg-docs/pmg-admin-guide.html#firewall_settings)

### Inbound

For the server to work, you must allow the following ports using your firewall:

  - For web access: 80, 443 (_443 can be GeoIP/sourceIP restricted, 80 needs to be open if you are using LetsEncrypt/ACME_)
  - Mailing basic: 25, 587, 465

### Outbound

**If you want** to filter outbound connections using your firewall - you will need to allow:

- Installation: while running the initial installation => please allow 80/tcp 443/tcp+udp to ANY
- Basics:
  - DNS: 53/tcp+udp to 1.1.1.1 and 8.8.8.8 (_or whatever dns servers you are using_)
  - NTP: 123/udp to 0.debian.pool.ntp.org and 0.debian.pool.ntp.org
  - APT: 443/tcp+udp to deb.debian.org and security.debian.org (_or whatever main repository you are using_)
- Proxmox: 443/tcp+udp to download.proxmox.com and enterprise.proxmox.com (_tbc.._)
- Mailing: 25/tcp, 465/tcp, 587/tcp to ANY
- LetsEncrypt: 443/tcp+udp to acme-v02.api.letsencrypt.org and staging-v02.api.letsencrypt.org (_debug mode_)
- ClamAV: 443/tcp+udp to database.clamav.net
- SpamAssassin: 80/tcp, 443/tcp+udp to spamassassin.apache.org and domains listed in the [mirror list](https://spamassassin.apache.org/updates/MIRRORED.BY)
- CloudMark Razor: 2703/tcp to discovery.razor.cloudmark.com and CURRENTLY 208.83.137.0/24
- to be continued..

## Public DNS

To filter incoming and outgoing mails, the Proxmox Mail Gateway needs to be configured as public mailserver!

If you are interested => here's a nice overview of SPF/DKIM/DMARC: [LINK](https://seanthegeek.net/459/demystifying-dmarc/)

**Needed**:

| TYPE | KEY                             | VALUE                                          | COMMENT                                                                                                                                                                                                                                       |
|:----:|:--------------------------------|:-----------------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  A   |  SRV.DOMAIN.TLD                 | SRV-PUBLIC-IP                                  | -                                                                                                                                                                                                                                             |
|  MX  | DOMAIN.TLD                 | 10 SRV.DOMAIN.TLD                              | -                                                                                                                                                                                                                                             |
| TXT  | DOMAIN.TLD                 | v=spf1 mx -all                                 | -                                                                                                                                                                                                                                             |
| TXT  | _dmarc.DOMAIN.TLD          | v=DMARC1; p=quarantine; aspf=s; adkim=s;       | You can also add a dedicated mail user to receive DMARC reports. See the 'overview' above for details. It would then look like this: 'v=DMARC1; p=quarantine; rua=mailto:ADDRESS@DOMAIN.TLD; ruf=mailto:ADDRESS@DOMAIN.TLD; aspf=s; adkim=s;' |
| TXT  | mail._domainkey.DOMAIN.TLD | v=DKIM1; p=MIIBIjANBgkqhkiG...                 | Replace the value by YOUR DKIM record! See: [Documentation](https://pmg.proxmox.com/pmg-docs/pmg-admin-guide.html#pmgconfig_mailproxy_dkim)                                                                                                   |
| TXT  | *.DOMAIN.TLD               | v=spf1 -all                                    | Any domain/subdomain that is not used to send mails, should IMPLICITLY DENY any senders!                                                                                                                                                      |
| PTR  | YOUR-SRV-IP | SRV.DOMAIN.TLD | You cannot set a PTR record in your DNS-Panel/management! Your internet provider/hoster has to do that. Bigger hosters will give you an option for this in their managment interface.                                                         |
