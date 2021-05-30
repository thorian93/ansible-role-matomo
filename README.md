# Ansible Role: Matomo

This role does a basic setup of Matomo on Debian and Ubuntu servers.

The configuration has to be done initially through the web interface.

[![Ansible Role: Matomo](https://img.shields.io/ansible/role/55137?style=flat-square)](https://galaxy.ansible.com/thorian93/matomo)
[![Ansible Role: Matomo](https://img.shields.io/ansible/quality/55137?style=flat-square)](https://galaxy.ansible.com/thorian93/matomo)
[![Ansible Role: Matomo](https://img.shields.io/ansible/role/d/55137?style=flat-square)](https://galaxy.ansible.com/thorian93/matomo)

## Known issues

None.

## Requirements

No special requirements; note that this role requires root access, so either run it in a playbook with a global `become: yes`, or invoke the role in your playbook like:

    - hosts: foobar
      roles:
        - role: ansible-role-matomo
          become: yes

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    matomo_version: ''

Set this to use an explicit version (e.g. `4.2.1`). Default installs the latest version.

    matomo_external_url: "{{ inventory_hostname }}"

The external URL under which Matomo will become available.

    matomo_use_proxy: false

Define, whether Matomo needs a proxy to access the internet.

    matomo_create_self_signed_cert: true
    matomo_self_signed_cert_subj: "/C=DE/ST=NRW/L=Cologne/O=Org/CN={{matomo_external_url}}"
    matomo_self_signed_certificate_key: "/etc/{{ apache2_http_name }}/ssl/matomo.key"
    matomo_self_signed_certificate: "/etc/{{ apache2_http_name }}/ssl/matomo.crt"

Configure self signed certificates.

    matomo_custom_cert: false
    matomo_custom_cert_file: /etc/{{ apache2_http_name }}/ssl/custom.crt
    matomo_custom_cert_key: /etc/{{ apache2_http_name }}/ssl/custom.key

Configure custom certificates.

    matomo_certificate_key: "{{ certbot_cert_path }}/privkey.pem"
    matomo_certificate: "{{ certbot_cert_path }}/cert.pem"
    matomo_certificate_chain: "{{ certbot_cert_path }}/fullchain.pem"

Configure certbot certificates.

    matomo_db_system: "mysql"
    matomo_db_name: "matomo"
    matomo_db_user: "matomo"
    matomo_db_pw: "matomo"

Configure database settings. Currently available is only MySQL/MariaDB. **Make sure to change the default user and password.**

    matomo_web_dir: "/var/www/matomo"

Define the webroot of Matomo.

    matomo_redirect_http_to_https: true

Configure whether Matomo should redirect all incoming requests to HTTPS per default.

    matomo_backup: false
    matomo_backup_dir: "/tmp/matomo"

Configure Backups fort Matomo.

    matomo_php_options:
      - line: "php_value open_basedir {{ matomo_web_dir }}:/usr/share/php:/usr/share/pear"
        regexp: "^php_value open_basedir"

Define PHP options for Matomo. The defaults given here are necessary for Matomo to work properly.

    matomo_mysql_options:
      - line: "max_allowed_packet = 64M"
        regexp: "^max_allowed_packet.*"

Define MySQL options for Matomo. The defaults given here are necessary for Matomo to work properly.

## Dependencies

  - [thorian93.ansible-role-apache2](https://galaxy.ansible.com/thorian93/apache2)
  - [thorian93.ansible-role-php](https://galaxy.ansible.com/thorian93/php)
  - [thorian93.ansible-role-certbot](https://galaxy.ansible.com/thorian93/certbot) - when no custom or self signed certificate is used
  - [geerlingguy.mysql](https://galaxy.ansible.com/geerlingguy/mysql)

## OS Compatibility

This role ensures that it is not used against unsupported or untested operating systems by checking, if the right distribution name and major version number are present in a dedicated variable named like `<role-name>_stable_os`. You can find the variable in the role's default variable file at `defaults/main.yml`:

    role_stable_os:
      - Debian 10
      - Ubuntu 18
      - CentOS 7
      - Fedora 30

If the combination of distribution and major version number do not match the target system, the role will fail. To allow the role to work add the distribution name and major version name to that variable and you are good to go. But please test the new combination first!

Kudos to [HarryHarcourt](https://github.com/HarryHarcourt) for this idea!

## Example Playbook

    ---
    - name: "Run role."
      hosts: all
      become: yes
      roles:
        - ansible-role-matomo

## Contributing

Please feel free to open issues if you find any bugs, problems or if you see room for improvement. Also feel free to contact me anytime if you want to ask or discuss something.

## Disclaimer

This role is provided AS IS and I can and will not guarantee that the role works as intended, nor can I be accountable for any damage or misconfiguration done by this role. Study the role thoroughly before using it.

## License

MIT

## Author Information

This role was created in 2020 by [Thorian93](http://thorian93.de/).