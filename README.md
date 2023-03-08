# PostgreSQL system role
![CI Testing](https://github.com/linux-system-roles/postgresql/workflows/tox/badge.svg)

The PostgreSQL system installs, configures, and starts the PostgreSQL server.

The role also optimizes the database server settings to improve performance.

The role currently works with PostgreSQL server 10 12 and 13.
## Role Variables
### postgresql_verison
You can set the version of PostgreSQL server to 10, 12, or 13.
```yaml
postgresql_version: "13"
```
### postgresql_password
Optionally, you can set a password for the `postgres` database superuser.
By default, no password is set, and a datababase is accessible from
the `postgres` system account through a UNIX socket.
It is recommended to encrypt the password using Ansible Vault.
```yaml
postgresql_password: !vault |
          $ANSIBLE_VAULT;1.2;AES256;dev
          ....
```
### postgresql_pg_hba_conf
The content of the `postgresql_pg_hba_conf` variable replaces the default
upstream configuration in the `/var/lib/pgsql/data/pg_hba.conf` file.
```yaml
postgresql_pg_hba_conf:
  - type: local
    database: all
    user: all
    auth_method: peer
  - type: host
    database: all
    user: all
    address: '127.0.0.1/32'
    auth_method: ident
  - type: host
    database: all
    user: all
    address: '::1/128'
    auth_method: ident
```
### postgresql_server_conf
The content of the `postgresql_server_conf` variable is
added to the end of the `/var/lib/pgsql/data/postgresql.conf` file.
As a result, the default settings are overwritten.
```yaml
postgresql_server_conf:
  ssl: on
  shared_buffers: 128 MB
  huge_pages: try
```
### postgresql_ssl_enable
To set up a SSL/TLS connection, set the `postgresql_ssl_enable` variable to `True`  and provide a server certificate and a private key.
```yaml
postgresql_ssl_enable: true
```
### postgresql_cert_name
In case you want to use own key and certificate. Use `postgresql_cert_name` variable. It's necessary to have both files in the same directory and with the same name with suffixes .crt and .key

Use `postgresql_cert_name` variable to specify certificate name.
For example your crt file is located in `/etc/certs/server.crt` and key in `/etc/certs/server.key`. So `postgresql_cert_name` value should be
```yaml
postgresql_cert_name: /etc/certs/server
```
### postgresql_certificates
This is a `list` of `dict` in the same format as used
by the `fedora.linux_system_roles.certificate` role.  Specify this variable if
you want the certificate role to generate the certificates for the PostgreSQL server
configured by the PostgreSQL role. With this example, `self-signed` certificate
`postgresql_cert.crt` is generated in `/etc/pki/tls/certs`.
Default to `[]`.
```yaml
postgresql_certificates:
  - name: postgresql_cert
    dns: ['localhost', 'www.example.com']
    ca: self-sign
```
### postgresql_input_file
To run an SQL script, define a path to your SQL file using the `postgresql_input_file` variable:
```yaml
postgresql_input_file: "/tmp/mypath/file.sql"
```
### postgresql_server_tuning
By default, the PostgreSQL system role enables server settings optimization
based on system resources. To disabe the tuning,
set the `postgresql_server_tuning` variable to `False`.
```yaml
postgresql_server_tuning: false
```

See the [`examples/`](examples) for details.

## Idempotention
This section should cover role behavior for repeated runs.
### Password change
Once the password is set using `postgresql_password` variable it isn't possible to
change it by setting other value. Also for every database acces using superuser must
be used `postgresql_password`. Including functionality of `postgresql_input_file`
### Config file redefinition
Config files generated from `postgresql_pg_hba_conf` and `postgresql_conf` are 
regenerated within each single run. So every change rewrite the
previous configuration.
### Version change
Once the postgresql server is installed it isn't possible upgrade the server by
increasing version number in `postgresql_version` also downgrade is not allowed.
### Server tunning
### SSL usage

## Example Playbook


```yaml
- hosts: all
  vars:
    postgresql_version: "13"
    postgresql_password: !vault |
          $ANSIBLE_VAULT;1.2;AES256;dev
          ....

  roles:
    - linux-system-roles.postgresql
```

You can find more examples in the [`examples/`](examples) directory.

## License

MIT
