<<<<<<< HEAD
# controller_configuration.global_vars
=======
# infra.aap_configuration.global_vars
>>>>>>> 05b830566dcc61b49195ee27848426b7a0a12574

## Description

An ansible role to define global variables that will be available to all of the
roles in the collection, if they are configured as follows:

```console
# tail -4 meta/main.yml

dependencies:
# List your role dependencies here, one per line. Be sure to remove the '[]' above,
# if you add dependencies to this list.
  - global_vars
```

## Provided Variables

This is currently providing the following variables:

| Variable Name | Default Value | Required | Description |
|:---|:---:|:---:|:---|
| `operation_translate` | [See the default value below](#operation_translate-default-value) | Yes | Provides translation from object states to human interpretation |

### operation_translate Default value

```yaml
---
operation_translate:
  present:
    verb: "Create/Update"
    action: "creation"
  absent:
    verb: "Remove"
    action: "deletion"
...
```

## License

<<<<<<< HEAD
[GPL-3.0](https://github.com/redhat-cop/controller_configuration#licensing)
=======
[GPL-3.0](https://github.com/redhat-cop/aap_configuration#licensing)
>>>>>>> 05b830566dcc61b49195ee27848426b7a0a12574

## Author

[Ivan Aragonés](https://github.com/ivarmu)
