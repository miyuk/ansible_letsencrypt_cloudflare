[![CircleCI](https://circleci.com/gh/miyuk/ansible_letsencrypt_cloudflare/tree/master.svg?style=svg)](https://circleci.com/gh/miyuk/ansible_letsencrypt_cloudflare/tree/master)

# Let's Encrypt Cloudflare

This is an Ansible role to issue Let's Encrypt wildcard certificate with using Cloudflare DNS.

## Requirements

This role has some dependencies to play.

- cryptography >= 1.2.3

## Role Variables

| Variable | Description | Default |
| --- | --- | --- |
| `certificate_directory` | ceritifcate and private key output directory | ./certs |
| `certificate_files_owner` | certificate and private key files owner | root |
| `certificate_files_group` | certificate and private key files group | root |
| `certificate_files_mode` | certificate and private key files permission | 0755 |
| `certificate_csr_filename` | output CSR filename | "{{ certificate_common_name }}.csr" |
| `certificate_crt_filename` | output certificate filename | "{{ certificate_common_name }}.crt" |
| `certificate_key_filename` | output private key filename | "{{ certificate_common_name }}.key" |
| `certificate_fullchain_filename` | output fullcation certificate filename | "{{ certificate_common_name }}.f`ullchain.pem" |
| `certificate_force_create_key` | if `yes` certificate file will recreate | no |
| `certificate_force_create_csr` | if `yes` CSR file will recreate | no |
| `certificate_force_create_crt` | if `yes` private key file will recreate | no |
| `certificate_remaining_days` | when last created certificate is expiring date left this, skip certificate creation | 30 |
| `certificate_key_size` | private key size | 2048 |
| `certificate_common_name` | certificate common name field | "{{ inventory_hostname }}" |
| `certificate_country_name` | certificate country name field (2-character) | - |
| `certificate_state_or_province_name` | certificate state field | - |
| `certificate_locality_name` | certificate locality name field | - |
| `certificate_organization_name` | certificate organization name field | - |
| `certificate_email_address` | certificate organaization name field | - |
| `certificate_subject_alt_name` | certificate SAN field. if this variable is undefined, Let's Encrypt will create certificate with `DNS:<Common Name>` and `DNS:*.<Common Name>`. | - |
| `letsencrypt_key_directory` | Let's Encrypt account key output directory | ./letsencrypt |
| `letsencrypt_key_filename` | Let's Encrtypt account key filename | account.key |
| `letsencrypt_key_size` | Let's Encrypt account key size | 2048 |
| `letsencrypt_email` | Let's Encrypt account email | '' |
| `letsencrypt_force_create_key` | if `yes` Let's Encrypt account key file will recreate | no |
| `cloudflare_email` | Cloudflare login email | '' |
| `cloudflare_api_key` | Cloudflare login API Key | '' |
| `cloudflare_domain` | Cloudflare attach domain | '' |

## Install

this module name is `letsencrypt_cloudflare`. So, you try to install below command under Ansible Playbook folder.

```bash
cd roles
git clone git@github.com:miyuk/ansible_letsencrypt_cloudflare.git letsencrypt_cloudflare
```

## Example Playbook

create certificate under `/etc/nginx/certs`. as `example.com`.

```yaml
---
- hosts: localhost
  connection: local
  vars:
    certificate_directory: /etc/nginx/certs
    certificate_csr_filename: "{{ certificate_common_name }}.csr"
    certificate_crt_filename: "{{ certificate_common_name }}.crt"
    certificate_key_filename: "{{ certificate_common_name }}.key"
    certificate_fullchain_filename: "{{ certificate_common_name }}.fullchain.pem"
    certificate_common_name: example.com
    letsencrypt_key_directory: ./letsencrypt
    letsencrypt_key_filename: account.key
    letsencrypt_email: admin@example.com
    cloudflare_email: admin@example.com
    cloudflare_api_key: '****************'
    cloudflare_domain: example.com
  roles:
    - ../..
```