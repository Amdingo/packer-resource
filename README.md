Currently in the process of including other post-processors (vagrant first)

# Packer Build Resource

A Concourse CI resource to build new [VMWare templates via Packer](https://www.packer.io/docs/post-processors/vsphere-template.html)

## Source Configuration
### Required

- `host (string)` - The vSphere host that contains the VM built by the vmware-iso.

- `password (string)` - Password to use to authenticate to the vSphere endpoint.

- `username (string)` - The username to use to authenticate to the vSphere endpoint.

### Optional

- `datacenter (string)` - If you have more than one, you will need to specify which one the ESXi used.

- `folder (string)` - Target path where the template will be created.

- `insecure (boolean)` - If it's true skip verification of server certificate. Default is false

## Behaviour

### `out`: Build a new VMWare template

#### Parameters
- `template`: *Required.* The path to the packer template.
- `var_file`: *Required.* The path or list of paths to a [external JSON variable file](https://www.packer.io/docs/templates/user-variables.html).

All other parameters will be passed through to packer as variables.

## Example

```yaml
resource_types:
- name: packer
  type: docker-image
  source:
    repository: amdingo/vmware-packer-resource

resources:
- name: build-ami
  type: packer
  source:
    host: "vsphere.mycompany.com"
    username: "..."
    password: "..."
    datacenter: "datacenter1"
    folder: "/packer-templates/os/distro-1"
    insecure: true
    
jobs:
- name: my-template
  plan:
  - put: build-template
    params:
      template: vmware_packer_template.json
      var_file:
         - secrets.json
         - foo.json
  ```
