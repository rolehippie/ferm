# ferm

[![Source Code](https://img.shields.io/badge/github-source%20code-blue?logo=github&amp;logoColor=white)](https://github.com/rolehippie/ferm)
[![General Workflow](https://github.com/rolehippie/ferm/actions/workflows/general.yml/badge.svg)](https://github.com/rolehippie/ferm/actions/workflows/general.yml)
[![Readme Workflow](https://github.com/rolehippie/ferm/actions/workflows/docs.yml/badge.svg)](https://github.com/rolehippie/ferm/actions/workflows/docs.yml)
[![Galaxy Workflow](https://github.com/rolehippie/ferm/actions/workflows/galaxy.yml/badge.svg)](https://github.com/rolehippie/ferm/actions/workflows/galaxy.yml)
[![License: Apache-2.0](https://img.shields.io/github/license/rolehippie/ferm)](https://github.com/rolehippie/ferm/blob/master/LICENSE)
[![Ansible Role](https://img.shields.io/badge/role-rolehippie.ferm-blue)](https://galaxy.ansible.com/rolehippie/ferm)

Ansible role to install and configure ferm firewall.

## Sponsor

Building and improving this Ansible role have been sponsored by my current and previous employers like **[Cloudpunks GmbH](https://cloudpunks.de)** and **[Proact Deutschland GmbH](https://www.proact.eu)**.

## Table of content

- [Requirements](#requirements)
- [Default Variables](#default-variables)
  - [ferm_default_forward_policy](#ferm_default_forward_policy)
  - [ferm_default_input_policy](#ferm_default_input_policy)
  - [ferm_default_output_policy](#ferm_default_output_policy)
  - [ferm_default_weight](#ferm_default_weight)
  - [ferm_enabled](#ferm_enabled)
  - [ferm_extra_hooks](#ferm_extra_hooks)
  - [ferm_general_dirs](#ferm_general_dirs)
  - [ferm_general_hooks](#ferm_general_hooks)
  - [ferm_general_rules](#ferm_general_rules)
  - [ferm_group_dirs](#ferm_group_dirs)
  - [ferm_group_rules](#ferm_group_rules)
  - [ferm_host_dirs](#ferm_host_dirs)
  - [ferm_host_rules](#ferm_host_rules)
  - [ferm_raw_rules](#ferm_raw_rules)
- [Discovered Tags](#discovered-tags)
- [Dependencies](#dependencies)
- [License](#license)
- [Author](#author)

---

## Requirements

- Minimum Ansible version: `2.10`


## Default Variables

### ferm_default_forward_policy

Default policy for forward chain

#### Default value

```YAML
ferm_default_forward_policy: DROP
```

### ferm_default_input_policy

Default policy for input chain

#### Default value

```YAML
ferm_default_input_policy: DROP
```

### ferm_default_output_policy

Default policy for output chain

#### Default value

```YAML
ferm_default_output_policy: ACCEPT
```

### ferm_default_weight

Default weight for rule files

#### Default value

```YAML
ferm_default_weight: 50
```

### ferm_enabled

Generally enable or disable ferm

#### Default value

```YAML
ferm_enabled: true
```

### ferm_extra_hooks

List of extra hook scripts

#### Default value

```YAML
ferm_extra_hooks: []
```

### ferm_general_dirs

List of general directories for chains

#### Default value

```YAML
ferm_general_dirs:
  - /etc/ferm/before.d
  - /etc/ferm/ferm.d
  - /etc/ferm/input.d
  - /etc/ferm/output.d
  - /etc/ferm/forward.d
  - /etc/ferm/pre.d
  - /etc/ferm/post.d
  - /etc/ferm/flush.d
```

### ferm_general_hooks

List of general hook scripts

#### Default value

```YAML
ferm_general_hooks: []
```

### ferm_general_rules

List of general rule definitions

#### Default value

```YAML
ferm_general_rules:
  - name: ssh
    weight: 20
    type: input
    content: |
      proto tcp dport ssh ACCEPT;
```

### ferm_group_dirs

List of group directories for chains

#### Default value

```YAML
ferm_group_dirs: []
```

### ferm_group_rules

List of group rule definitions

#### Default value

```YAML
ferm_group_rules: []
```

### ferm_host_dirs

List of host directories for chains

#### Default value

```YAML
ferm_host_dirs: []
```

### ferm_host_rules

List of host rule definitions

#### Default value

```YAML
ferm_host_rules: []
```

### ferm_raw_rules

Raw rules applied with ferm

#### Default value

```YAML
ferm_raw_rules: |
  @include before.d/;

  domain (ip ip6) {
    table filter {
      chain INPUT {
        policy {{ ferm_default_input_policy }};

        mod state state INVALID DROP;
        mod state state (ESTABLISHED RELATED) ACCEPT;

        interface lo ACCEPT;
        proto icmp ACCEPT;

        @include input.d/;
      }

      chain FORWARD {
        policy {{ ferm_default_forward_policy }};

        mod state state INVALID DROP;
        mod state state (ESTABLISHED RELATED) ACCEPT;

        @include forward.d/;
      }

      chain OUTPUT {
        policy {{ ferm_default_output_policy }};
        @include output.d/;
      }
    }
  }

  @include ferm.d/;
```

## Discovered Tags

**_ferm_**


## Dependencies

- None

## License

Apache-2.0

## Author

[Thomas Boerger](https://github.com/tboerger)
