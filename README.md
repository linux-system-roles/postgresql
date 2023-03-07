# PostgreSQL system role
![CI Testing](https://github.com/linux-system-roles/postgresql/workflows/tox/badge.svg)

This role installs, configures, and starts PostgreSQL Server.

The role also optimizes the database server settings to improve performance.

The role currently works with PostgreSQL server 10 12 and 13.
## Role Variables
### postgresql_verison
Allow set up version of Postgresql server. This role supports Postgresql 10 12 and 13
```yaml
postgresql_version: "13"
```
### postgresql_password
Optionally, you can set up password for database super user `postgres` by default
there is not a password, datababase is accessible from `postgres` system account via UNIX socket.
users are encouraged to use ansible vault
```yaml
postgresql_password: !vault |
          $ANSIBLE_VAULT;1.2;AES256;dev
          ....
```
### postgresql_pg_hba_conf
A description of input variables that are not reqiured. Upstream configuration is used by default.
Usage of `postgresql_pg_hba_conf` causes replacement of default upstream configuration
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
Usage of `postgresql_server_conf` adds defined values at the end of postgresql.conf.
So the default ones are overwritten.
```yaml
postgresql_server_conf:
  ssl: on
  shared_buffers: 128 MB
  huge_pages: try
```
### postgresql_ssl_enable
To set up ssl connection it's necessary to set up `postgresql_ssl_enable` variable and provide server certificate and key.
```yaml
postgresql_ssl_enable: true
```
### postgresql_cert_name
In case you want to use own key and certificate. Use `postgresql_cert_name` variable. It's necessary to have both files in the same directory and with the same name with suffixes .crt and .key

To specify certificate name use `postgresql_cert_name` variable.
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
For running SQL script define path to your SQL file using `postgresql_input_file`:
```yaml
postgresql_input_file: "/tmp/mypath/file.sql"
```
### postgresql_server_tuning
By default the system role makes server settings tuning based on system resources,
This functionality is enabled by default. For disabling it there is a possibility to
set up the `postgresql_server_tuning` variable.
```yaml
postgresql_server_tuning: false
```

More about usage could be found in [`examples/`](examples) directory

## Idempotention
This section should cover role behavior for repeated runs.
### Password change
Once the password is set using `postgresql_password` variable it isn`t possible to
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
