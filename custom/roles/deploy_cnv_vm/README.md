# deploy_cnv_vm

Deploys VirtualMachines directly from OpenShift Virtualization DataSources without intermediate PVCs.

## Purpose

This role simplifies VM deployment by:
- Creating VMs directly from DataSources (no intermediate PVC in cnv-images needed)
- Handling DataVolume creation automatically
- Creating LoadBalancer services for VM access
- Waiting for VMs to be running

## Advantages over cnv_instances

- No credential context issues - single atomic operation
- Uses DataSources directly (no intermediate PVC)
- Simpler configuration
- Built-in LoadBalancer service creation
- Automatic storage class handling

## Example Usage

```yaml
deploy_cnv_vm_namespace: "aap-lab"

deploy_cnv_vm_instances:
  - name: aap-lab-rhel10
    datasource_name: rhel10
    cores: 4
    memory: 16Gi
    disk_size: 100Gi
    cloud_init_user: lab-user
    cloud_init_password: "{{ common_admin_password }}"
    ports:
      - port: 22
        protocol: TCP
        targetPort: 22
        name: ssh
      - port: 443
        protocol: TCP
        targetPort: 443
        name: https
      - port: 80
        protocol: TCP
        targetPort: 80
        name: http
```

## Integration with AgnosticV

Replace `agnosticd.cloud_vm_workloads.cnv_instances` and `ocp.custom.create_boot_source_pvc`:

```yaml
workloads:
  - agnosticd.core_workloads.ocp4_workload_openshift_virtualization
  - agnosticd.core_workloads.ocp4_workload_metallb
  - ocp.custom.create_namespace
  - ocp.custom.deploy_cnv_vm  # <- Replaces both boot_source_pvc and cnv_instances
```
