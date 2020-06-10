# Ansible Role: consul_template

This role installs and configures consul-template on systemd-enabled Linux
systems.

The overall aim of this role is to do one thing and do it well - which in this
case is installing the consul-template binary and systemd service unit.

The following items are **not** what this role will do:
* Distribute or create consul-template templates
* Allow for heavy tweaking and customization of default options
* Do insanely complicated things with loops within loops within loops...

What really makes this role unique compared to others (the ones I have seen
anyway) is that it configures consul-template as a systemd "instantiated
service" (see
[here](https://www.freedesktop.org/software/systemd/man/systemd.service.html)
or `man 5 systemd.service` for details).

Configuring the consul-template service in this way means that you can
effortlessly launch multiple consul-template systemd services that all use
different configuration files.

For example, if you wanted to use a
consul-template configuration file located at
`/etc/consul-template.d/consul.hcl`, you could easily launch it through the
following command:

```sh
$ sudo systemctl start consul-template@consul
```

This means that consul-template is run as a templated service, so you can have
multiple consul-template instances templated from the same systemd service
template and they can all be templating completely different consul-template
templates that could theoretically be consul-template templates for other
templated consul-templates. Or whatever.

## Requirements

Target hosts can only be Linux systems using systemd. Otherwise just `unzip` on
your Ansible controller.

You will also need a working Consul or Vault environment that your target hosts
are joined/have access to in order to use consul-template. But you wouldn't be
here without one of those anyway (right?).

## Dependencies

None.

## Example Playbook

    - hosts: example-hosts
      become: yes
      roles:
        - role: brickstool.consul_template

## License

MIT / BSD

## Author Information

Created by Samuel Noordhuis in 2020. Inspired heavily by the Ansible roles and
writings of [Jeff Geerling](https://github.com/geerlingguy).

If you see any errors or think this role could be improved in some way, you
are welcome to open an issue/feature request or create a pull request :)

## TO-DO

1. Add molecule tests
