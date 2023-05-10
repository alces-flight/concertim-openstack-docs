## Requirements

- Existing OpenStack Installation
- OpenStack CLI Tools Installed (both `python-openstackclient` and `python-heatclient`)
- OpenStack RC File (sourced to shell environment where creation scripts will be run from) 
- [Flight Solo 2023.2+](https://repo.openflighthpc.org/?prefix=images/FlightSolo/) Image in OpenStack

## How To

- Set cluster information in `hpc-cores.sh`
    - **Ensure that the corresponding private key to `SSH_KEY` is automatically used by your local `ssh` config as the create-hpc-core.sh script needs to login to the gateway node before moving on**
- Launch HPC core
  ```shell
  bash create-hpc-core.sh
  ```
- Set node information in `compute-nodes.sh`
- Launch either a set number of nodes or a scaling group that varies between 1 and the set `NODE_COUNT`
    - A) Launch set number of compute nodes
      ```shell
      bash create-compute-nodes.sh
      ```
    - B) Launch autoscaling group of compute nodes
      ```shell
      bash create-autoscale-compute-nodes.sh
      ```

Note: All scripts are created such that they can be run independently.

## Autoscaling

To change the number of nodes currently up in the autoscaling group, run:
```shell
bash modify-autoscale-compute-nodes.sh X
```

Where X is the desired number of nodes. OpenStack will then create/delete instances to reach this target. 

## Known Errors

The cluster building blocks are assuming a managed openstack cloud provided by Alces Flight - however it is possible to run on a standard openstack deployment. In doing so you may run into the following issues:

### DNS Domain Variable

When trying to build the HPC core components, it is possbile to run into an error where `dns_domain` does not resolve to anything and an exception is thrown. This is fixed by deleteing/commenting out the line in the [hpc-core.yaml](https://github.com/openflighthpc/cluster-building-blocks/blob/main/hpc-core.yaml) file:

```
  cluster-network:
    type: OS::Neutron::Net
    properties:
      name: { list_join: ['-', [{ get_param: clustername }, 'network']] }
#      dns_domain: { list_join: ['.', [{ get_param: clustername }, 'alces.network.']] }
```
