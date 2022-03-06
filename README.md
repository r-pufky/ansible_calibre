# Calibre
Calibre-Web using Calibre as a backend, with optional feature support.

## Requirements
The Calibre binaries themselves have security considerations to make and should
be run in an untrusted environment:

[vars/calibre.yml](https://github.com/r-pufky/ansible_calibre/blob/main/vars/main/calibre.yml).

## Role Variables
Settings have been throughly documented for usage. Extensive configuration
information is contained in:

[defaults/main/main.yml](https://github.com/r-pufky/ansible_calibre/blob/main/defaults/main/main.yml).

## Dependencies
N/A

## Example Playbook
host_vars/calibre.example.com/vars/calibre.yml
``` yaml
calibre_create_user: true
calibre_library:     '/data/library'
# Calibre-Web root user. This user will automatically be created and have root.
calibre_root_name:     'example-admin-user'
calibre_root_email:    ''
calibre_root_password: '{{ vault_calibre_root_password }}'
```

site.yml
``` yaml
- name:   'calibre server'
  hosts:  'calibre.example.com'
  become: true
  roles:
    - 'r_pufky.calibre'
```

## Reverse Proxy
Calibre should be run behind a reverse proxy given the security concerns.

NGINX
``` nginx
upstream books-backend {
  server {{ calibre_host }}:{{ calibre_server_port }};
}

server {
  listen 443 ssl http2;
  server_name books.{{ domain }} books;

  client_max_body_size 20M;
  location / {
    proxy_bind       $server_addr;
    proxy_set_header Host            $http_host;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Scheme        $scheme;
    proxy_pass http://books-backend;
  }
}
```

## Issues
Create a bug and provide as much information as possible.

Associate pull requests with a submitted bug.

## License
[AGPL-3.0 License](https://github.com/r-pufky/ansible_calibre/blob/main/LICENSE)

## Author Information
https://keybase.io/rpufky

