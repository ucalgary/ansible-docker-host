# Docker Host

This role installs and configures Docker CE on Red Hat Enterprise Linux and CentOS 7 hosts. It also installs firewalld services and TLS certificates for Docker.

## Requirements

This role requires Ansible 2.2 or higher, and platform requirements are listed in the metadata file.

## Role Variables

The variables that can be passed to this role and a brief description about them are as follows.

    # The Docker edition: docker-ce or docker-ee
    # For docker-ce, a variant can be specified: stable, edge, test
    docker_edition: 'docker-ce'
    docker_ce_variant: 'stable'

    # Host path to install TLS certificate files
    docker_certs_dir: '~/docker'

    # Source paths for TLS certificate files
    docker_tlscacert: 'files/ca.pem'
    docker_tlscert: 'cert.pem'
    docker_tlskey: 'key.pem'

    # Enable the Docker remote API and firewalld service
    # 2375/tcp, 2376/tcp
    enable_remote_api: true

    # Enable firewalld service for overlay networks
    # 4789/udp, 7946/tcp, 7946/udp
    enable_swarm_overlay_networks: true

    # Enable firewalld service for default published service ports
    # 30000-32767/tcp
    enable_swarm_service_ports: true

    # Enable firewalld service for swarm manager ports
    # 2377/tcp
    enable_swarm_manager_ports: true

    # Disable contacting legacy registries
    dockerd_disable_legacy_registry: true

In addition to the defined variables, this role will also process any variable starting with `dockerd_` into Docker daemon options, writing them into a `daemon.json` configuration file. Variables are processed by removing the leading `dockerd_` and replacing `_` characters with `-`. The following is an example of `dockerd_` options and the resulting `daemon.json` configuration.

**Playbook Variables:**

    dockerd_disable_legacy_registry: true
    dockerd_experimental: true
    dockerd_hosts:
      - unix:///var/run/docker.sock
      - tcp://0.0.0.0:2376
    dockerd_metrics_addr: 0.0.0.0:9323

**Daemon Configuration:**

    {
        "disable-legacy-registry": true,
        "experimental": true,
        "hosts": [
            "unix:///var/run/docker.sock",
            "tcp://0.0.0.0:2376"
        ],
        "metrics-addr": "0.0.0.0:9323"
    }

## Example Playbook

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }

## License

Apache 2

## Author Information

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
