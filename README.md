# work

[![Source Code](https://img.shields.io/badge/github-source%20code-blue?logo=github&logoColor=white)](https://github.com/rolehippie/ferm) [![Testing Build](https://github.com/rolehippie/ferm/workflows/testing/badge.svg)](https://github.com/rolehippie/ferm/actions?query=workflow%3Atesting) [![Readme Build](https://github.com/rolehippie/ferm/workflows/readme/badge.svg)](https://github.com/rolehippie/ferm/actions?query=workflow%3Areadme) [![Galaxy Build](https://github.com/rolehippie/ferm/workflows/galaxy/badge.svg)](https://github.com/rolehippie/ferm/actions?query=workflow%3Agalaxy) [![License: Apache-2.0](https://img.shields.io/github/license/rolehippie/ferm)](https://github.com/rolehippie/ferm/blob/master/LICENSE) 

Ansible role to install and configure ferm firewall. 

## Sponsor 

[![Proact Deutschland GmbH](https://proact.eu/wp-content/uploads/2020/03/proact-logo.png)](https://proact.eu) 

Building and improving this Ansible role have been sponsored by my employer **Proact Deutschland GmbH**.

## Table of content

* [Default Variables](#default-variables)
  * [ferm_default_forward_policy](#ferm_default_forward_policy)
  * [ferm_default_input_policy](#ferm_default_input_policy)
  * [ferm_default_output_policy](#ferm_default_output_policy)
  * [ferm_default_weight](#ferm_default_weight)
  * [ferm_enabled](#ferm_enabled)
  * [ferm_extra_hooks](#ferm_extra_hooks)
  * [ferm_general_dirs](#ferm_general_dirs)
  * [ferm_general_hooks](#ferm_general_hooks)
  * [ferm_general_rules](#ferm_general_rules)
  * [ferm_group_dirs](#ferm_group_dirs)
  * [ferm_group_rules](#ferm_group_rules)
  * [ferm_host_dirs](#ferm_host_dirs)
  * [ferm_host_rules](#ferm_host_rules)
  * [ferm_raw_rules](#ferm_raw_rules)
* [Dependencies](#dependencies)
* [License](#license)
* [Author](#author)

---

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
    content: "proto tcp dport ssh ACCEPT;\n"
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
ferm_raw_rules: "@include before.d/;\n\ndomain (ip ip6) {\n  table filter {\n    chain\
  \ INPUT {\n      policy {{ ferm_default_input_policy }};\n\n      mod state state\
  \ INVALID DROP;\n      mod state state (ESTABLISHED RELATED) ACCEPT;\n\n      interface\
  \ lo ACCEPT;\n      proto icmp ACCEPT;\n\n      @include input.d/;\n    }\n\n  \
  \  chain FORWARD {\n      policy {{ ferm_default_forward_policy }};\n\n      mod\
  \ state state INVALID DROP;\n      mod state state (ESTABLISHED RELATED) ACCEPT;\n\
  \n      @include forward.d/;\n    }\n\n    chain OUTPUT {\n      policy {{ ferm_default_output_policy\
  \ }};\n      @include output.d/;\n    }\n  }\n}\n\n@include ferm.d/;\n"
```

## Dependencies

* None

## License

Apache-2.0

## Author

[Thomas Boerger](https://github.com/tboerger)
