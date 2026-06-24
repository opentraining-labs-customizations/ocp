# create_boot_source_pvc

Creates PersistentVolumeClaims in cnv-images namespace from OpenShift Virtualization DataSources.

## Purpose

This role creates boot source PVCs that can be used by the cnv_instances workload. The cnv_instances workload expects boot images to exist as PVCs in the cnv-images namespace. This role clones DataSources from openshift-virtualization-os-images into PVCs.

## Requirements

- OpenShift cluster with OpenShift Virtualization installed
- DataSources available in openshift-virtualization-os-images namespace
- kubernetes.core collection

## Role Variables

### Required Variables

- `boot_source_pvcs`: List of PVC definitions to create

### Optional Variables

- `boot_source_pvc_namespace`: Namespace for PVCs (default: "cnv-images")
- `boot_source_create_namespace`: Create namespace if missing (default: true)
- `boot_source_default_storage`: Default PVC size (default: "30Gi")
- `boot_source_default_access_modes`: Default access modes (default: ["ReadWriteMany"])
- `boot_source_default_volume_mode`: Default volume mode (default: "Block")
- `boot_source_default_datasource_namespace`: DataSource namespace (default: "openshift-virtualization-os-images")

## Example Usage

```yaml
boot_source_pvcs:
  - name: rhel9
    datasource_name: rhel9
    storage: "30Gi"
  
  - name: rhel8
    datasource_name: rhel8
    storage: "30Gi"
    
  - name: fedora
    datasource_name: fedora
    storage: "20Gi"
    access_modes:
      - ReadWriteMany
    volume_mode: Block
```

## Integration with AgnosticV

Add to workloads list BEFORE cnv_instances:

```yaml
workloads:
  - agnosticd.core_workloads.ocp4_workload_openshift_virtualization
  - ocp.custom.create_boot_source_pvc   # <-- Add this
  - agnosticd.cloud_vm_workloads.cnv_instances
```

Configure boot sources:

```yaml
boot_source_pvcs:
  - name: rhel9
    datasource_name: rhel9
```

Then use in cnv_instances:

```yaml
cnv_instances:
  - name: "my-vm"
    image: "rhel9"  # References the PVC created above
    memory: "20Gi"
    cores: 4
```

## Author

Created for RHDP AgnosticV catalog deployments.
