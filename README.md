# ferm

[![Source Code](https://img.shields.io/badge/github-source%20code-blue?logo=github&logoColor=white)](https://github.com/rolehippie/ferm) [![Build Status](https://img.shields.io/drone/build/rolehippie/ferm/master?logo=drone)](https://cloud.drone.io/rolehippie/ferm) [![License: Apache-2.0](https://img.shields.io/github/license/rolehippie/ferm)](https://github.com/rolehippie/ferm/blob/master/LICENSE) 

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
  * [ferm_general_hooks](#ferm_general_hooks)
  * [ferm_general_rules](#ferm_general_rules)
  * [ferm_group_rules](#ferm_group_rules)
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

### ferm_group_rules

List of group rule definitions

#### Default value

```YAML
ferm_group_rules: []
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
