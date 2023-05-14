# Concertim-Openstack Documents Repo

The purpose of this repo is to manage the docs for all services related to Concertim and Openstack Cluster Portal.

The docs are stored here for quick reference, but they are also present in their respective repositories:
- [Concertim Visualization App](https://github.com/alces-flight/concertim-ct-visualisation-app)
- [Concertim Metric Reporting and Processing Daemon](https://github.com/alces-flight/concertim-metric-reporting-daemon)
- [Concertim Ansible](https://github.com/alces-flight/concertim-ansible-playbook)
- [Concertim-Openstack Service Daemon](https://github.com/alces-flight/concertim-openstack-service)

## Concertim

For docs pertaining to Concertim Environment:

- [Concertim Installation/Setup](concertim-install.md)
- [Concertim Ansible Deployment](concertim-ansible.md)
- [Concertim Metric Reporting Daemon](concertim-metric-daemon.md)
   - [Usage/Examples](concertim-metric-examples.md)
- [Concertim Visualization App](concertim-ui.md)
   - [Usage/Examples](concertim-ui-examples.md)

## Openstack

For docs pertaining to Openstack Environment:

- [Concertim Openstack Service](concertim-os-service.md)


## Launching a Cluster

When running the `concertim-openstack-service` to gather information and push to concertim, we recommend building clusters using the [FlightSolo Cluster Builder](https://github.com/openflighthpc/cluster-building-blocks) to launch a cluster in your Openstack environment. 

- [FlightSolo Cluster Builder docs](openflight-cluster-creation.md)

It is also possible to launch individual instances/clusters using other means, however the name of the instance name must follow the format `<instance_name>.<cluster_name>.<anything_else>` for the concertim-openstack-service to recognize the cluster. An example would be `storage001.examplecluster.234hf` for the `storage001` instance in the cluster named `examplecluster` with a UID of `234hf`. Note, the UID is NOT needed, but extra metadata can be added to the end of the name.
