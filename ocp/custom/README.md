# OCP Custom Collection

Custom OpenShift utilities collection for namespace and resource management.

**Namespace:** `ocp`  
**Collection:** `custom`  
**Full name:** `ocp.custom`

## Roles

### create_namespace

Creates an OpenShift namespace/project.

**Variables:**
- `create_namespace_name`: Name of the namespace to create (required)
- `create_namespace_display_name`: Display name for the namespace (optional)
- `create_namespace_description`: Description for the namespace (optional)

**Example:**
```yaml
- hosts: localhost
  roles:
    - role: ocp.custom.create_namespace
      create_namespace_name: aap-lab
      create_namespace_display_name: "AAP 2.7 Lab Environment"
```
