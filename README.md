# PostgreSQL system role

[![ansible-lint.yml](https://github.com/linux-system-roles/postgresql/actions/workflows/ansible-lint.yml/badge.svg)](https://github.com/linux-system-roles/postgresql/actions/workflows/ansible-lint.yml) [![ansible-test.yml](https://github.com/linux-system-roles/postgresql/actions/workflows/ansible-test.yml/badge.svg)](https://github.com/linux-system-roles/postgresql/actions/workflows/ansible-test.yml) [![codespell.yml](https://github.com/linux-system-roles/postgresql/actions/workflows/codespell.yml/badge.svg)](https://github.com/linux-system-roles/postgresql/actions/workflows/codespell.yml) [![markdownlint.yml](https://github.com/linux-system-roles/postgresql/actions/workflows/markdownlint.yml/badge.svg)](https://github.com/linux-system-roles/postgresql/actions/workflows/markdownlint.yml) [![qemu-kvm-integration-tests.yml](https://github.com/linux-system-roles/postgresql/actions/workflows/qemu-kvm-integration-tests.yml/badge.svg)](https://github.com/linux-system-roles/postgresql/actions/workflows/qemu-kvm-integration-tests.yml) [![tft.yml](https://github.com/linux-system-roles/postgresql/actions/workflows/tft.yml/badge.svg)](https://github.com/linux-system-roles/postgresql/actions/workflows/tft.yml) [![tft_citest_bad.yml](https://github.com/linux-system-roles/postgresql/actions/workflows/tft_citest_bad.yml/badge.svg)](https://github.com/linux-system-roles/postgresql/actions/workflows/tft_citest_bad.yml) [![woke.yml](https://github.com/linux-system-roles/postgresql/actions/workflows/woke.yml/badge.svg)](https://github.com/linux-system-roles/postgresql/actions/workflows/woke.yml)

The PostgreSQL system role installs, configures, and starts the PostgreSQL
server.

The role also optimizes the database server settings to improve performance.

## Requirements

The role currently works with the PostgreSQL server 10, 12, 13, 15 and 16.

### Collection requirements

The role requires some external collections.  Use this to install them:

```bash
ansible-galaxy collection install -vv -r meta/collection-requirements.yml
```

## Role Variables

### postgresql_version

You can set the version of the PostgreSQL server to 10, 12, 13, 15 or 16.

```yaml
postgresql_version: "13"
```

### postgresql_password

Optionally, you can set a password for the `postgres` database superuser.
By default, no password is set, and a datababase is accessible from the
`postgres` system account through a UNIX socket. It is recommended to encrypt
the password by using Ansible Vault.

```yaml
postgresql_password: !vault |
          $ANSIBLE_VAULT;1.2;AES256;dev
          ....
```

This option is not available for running the role in container builds.

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

The content of the `postgresql_server_conf` variable is added to the end of
the `/var/lib/pgsql/data/postgresql.conf` file. As a result, the default
settings are overwritten.

```yaml
postgresql_server_conf:
  ssl: on
  shared_buffers: 128MB
  huge_pages: try
```

### postgresql_ssl_enable

To set up an SSL/TLS connection, set the `postgresql_ssl_enable` variable to
`true`  and provide a server certificate and a private key.

```yaml
postgresql_ssl_enable: true
```

### postgresql_cert_name

If you want to use your own certificate and private key, use the
`postgresql_cert_name` variable to specify the certificate name. You must keep
both certificate and key files in the same directory and under the same name
with the `.crt` and `.key` suffixes on the managed node. The value should be an
absolute path.

For example, if your certificate file is located in `/etc/certs/server.crt` and
your private key in `/etc/certs/server.key`, set the `postgresql_cert_name`
value to:

```yaml
postgresql_cert_name: /etc/certs/server
```

### postgresql_certificates

The `postgresql_certificates` variable requires a `list` of `dict` in the same
format as used by the `fedora.linux_system_roles.certificate` role. Specify the
`postgresql_certificates` variable if you want the certificate role to generate
certificates for the PostgreSQL server configured by the PostgreSQL role.
In the following example, a `self-signed` certificate `postgresql_cert.crt` is
generated in the `/etc/pki/tls/certs/` directory. By default, no certificates
are automatically generated (`[]`).

```yaml
postgresql_certificates:
  - name: postgresql_cert
    dns: ['localhost', 'www.example.com']
    ca: self-sign
```

### postgresql_input_file

To run an SQL script, define a path to your SQL file by using the
`postgresql_input_file` variable:

```yaml
postgresql_input_file: "/tmp/mypath/file.sql"
```

This option is not available for running the role in container builds.

### postgresql_server_tuning

By default, the PostgreSQL system role enables server settings optimization
based on system resources. To disable the tuning, set the
`postgresql_server_tuning` variable to `false`.

```yaml
postgresql_server_tuning: false
```

However, when running against a
[buildah connection](https://docs.ansible.com/ansible/latest/collections/containers/podman/buildah_connection.html),
i.e. a container build, this defaults to `false`. The available RAM during a
container build is independent of the actual deployment.

See the [`examples/`](examples) for details.

## Idempotence

This section should cover role behavior for repeated runs.

### Password change

Once you set the password by using the `postgresql_password` variable, it is
impossible to change the password by setting another value. You must use the
`postgresql_password` variable for every database access under the superuser,
including running an SQL script (the functionality of the
`postgresql_input_file` variable).

### Config file redefinition

Configuration files generated from `postgresql_pg_hba_conf` and `postgresql_conf`
are regenerated within each single run. Therefore, every change rewrites the
previous configuration.

### Version change

Once the PostgreSQL server is installed, it is impossible to upgrade or
downgrade the server by increasing or decreasing the version number in the
`postgresql_version` variable.

### Server tuning

This option reflects the setup of the latest run of the role.

### SSL usage

This option reflects the setup of the latest run of the role. The PostgreSQL
server needs properly defined certificates and keys to run with enabled SSL/TLS.

## Example Playbook

```yaml
- name: Manage postgres
  hosts: all
  vars:
    postgresql_version: "13"
    postgresql_password: !vault |
          $ANSIBLE_VAULT;1.2;AES256;dev
          ....
  roles:
    - linux-system-roles.postgresql
```

You can find more examples in the [`examples/`](examples) directory.

## rpm-ostree

NOTE: By default, `get_ostree_data.sh` will return the packages for the default
version of PostgreSQL.  You will need to amend the output if you want to use a
different version - e.g. change `@postgresql:13/server` to
`@postgresql:15/server`

See README-ostree.md for more information.

## License

MIT
