# Ansible Role: consul_template

This role installs and configures consul-template on systemd-enabled Linux
systems.

The overall aim of this role was to do one thing and one thing well - which in
this case is installing the consul-template binary and systemd service unit.
There are no consul-template templates created or distributed, no weird and
difficult-to-follow task loops, and no exceedingly long list of variables and
defaults to use (in fact there are only a handful right now, with the
possibility of slightly increasing that number in the future).

However, what makes this role really stand out from similar roles (based on
what I've been able to find online anyway) is that it configures
consul-template as a systemd "instantiated service" (see
[here](https://www.freedesktop.org/software/systemd/man/systemd.service.html)
or `man 5 systemd.service` for details).

The systemd unit file in this repo is configured to launch consul-template with
the `-config` parameter pointing to `/etc/consul-template.d/%I.hcl` - where
`%I` is whatever string you pass to the service after the `@` but before the
`.service` (`systemctl` assumes that you are referring to a service unless you
specify otherwise). Assuming `/etc/consul-template.d/consul.hcl` exists, an
instantiated consul-template service could be started like so:

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
