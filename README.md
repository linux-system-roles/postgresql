# PostgreSQL system role
![CI Testing](https://github.com/linux-system-roles/template/workflows/tox/badge.svg)

This role installs, configures, and starts PostgreSQL Server.

The role also optimizes the datadase server settings to improve performance.

The role currently works with PostgreSQL server 10 12 and 13.
## Role Variables
### postgresql\_verison
Allow set up version of Postgresql server. This role supports Postgresql 10 12 and 13
```yaml
postgresql_version: "13"
```
### postgresql\_password
Set up default password for database super user `postgres`
```yaml
postgresql_password: "mysecretpassword"
```
### pg\_hba\_conf
A description of input variables that are not reqiured. Upstream configuration is used by default.
Usage of `pg_hba_conf` causes replacement of default upstream configuration
```yaml
pg_hba_conf:
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
### postgresql\_server\_conf
Usage of `postgresql_server_conf` adds defined values at the end of postgresql.conf.
So the default ones are overwritten.
```yaml
postgresql_server_conf:
  ssl: on
  shared_buffers: 128 MB
  huge_pages: try
```
### ssl\_enable
To set up ssl connection it's necessary to set up `ssl_enable` variable and provide server certificate and key.
```yaml
ssl_enable: yes
```
### cert\_name
To specify certificate name use `cert_name` variable.
You can copy your certificate to `/etc/pki/tls/certs/server.crt` and key to `/etc/pki/tls/private/server.key` or
you can also use certificate system role. For more detail see [`examples/`](examples).
```yaml
cert_name: "server"
```
### postgresql\_input\_file
For running SQL script define path to your SQL file using `postgresql_input_file`:
```yaml
postgresql_input_file: "/tmp/mypath/file.sql"
```

More about usage could be found in [`examples/`](examples) directory


## Example Playbook


```yaml
- hosts: all
  vars:
    postgresql_version: "13"
    postgresql_password: "passwd"

  roles:
    - linux-system-roles.postgresql
```

More examples can be provided in the [`examples/`](examples) directory. These
can be useful especially for documentation.

## License

MIT
