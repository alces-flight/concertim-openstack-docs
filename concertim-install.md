# Concertim Installation and Configuration

Concertim is a visualisation tool for virtual clusters. It is used to visualise instances and metrics collected for those instances. At a high level it consists of the following parts:

* The Concertim UI. A web application that visualises the configured racks, their instances and their reported metrics.
* Concertim OpenStack service, an OpenStack adapter for the Concertim UI. It interrogates an OpenStack installation, configures the Concertim UI with the instances it finds; interrogates OpenStack for metrics about those instances and reports them to the Concertim UI.
* Concertim Cluster Builder.  A web service that receives requests to build clusters from the Concertim UI and orchestrates the building of those clusters on an OpenStack installation.


## Concertim UI installation

Concertim UI is intended to be installed as a number of Docker images.

The most recent version of the Concertim UI is `v0.2.1`. To install this version follow the instructions at https://github.com/alces-flight/concertim-ansible-playbook/blob/v0.2.1/concertim-ui/README.md. The instructions detail how to setup access to a private docker registry containing the concertim docker images.

The Docker images can be ran on any machine suitable for running Docker containers. This could be the same machine that runs an OpenStack installation, but it doesn't have to be.

The Docker container `concertim-ui_proxy_1` binds its HTTP and HTTPS ports to the Docker host's `80` and `443` ports respectively on interface `0.0.0.0`. Details on configuring the defaults can be found at https://github.com/alces-flight/concertim-ansible-playbook/blob/v0.2.1/concertim_ui/README.md#configuration.

The HTTP port redirects all traffic to the HTTPS port, as such access to it is optional.

Any users of the Concertim UI will need to be able to access the HTTPS port of the Docker container `concertim-ui_proxy_1` and optionally the HTTP port.  By default bound to port `443` on the docker host.

The Concertim OpenStack service will need to be able to access the HTTPS port of the `concertim-ui_proxy_1` container. By default bound to port `443` on the docker host.

Concertim UI needs to be able to make HTTP/s requests to both Concertim OpenStack service and to Concertim Cluster Builder.


## Concertim OpenStack service installation

Instructions for installing and configuring the Concertim OpenStack service is given in its README https://github.com/alces-flight/concertim-openstack-service/tree/main

It could be installed on the same machine that is running the OpenStack installation but it doesn't have to.

It needs to be able to make HTTP/s requests to the OpenStack installation that it is to monitor and to the Concertim UI.

## Concertim cluster builder service installation

The most recent version of Concertim Cluster Builder is v0.1.1.

Instructions for installing and configuring the Concertim Cluster Builder service is given in its README https://github.com/alces-flight/concertim-cluster-builder/blob/v0.1.1/README.md.  It is intended to be ran as a docker container.

It needs to be able to receive HTTP/s requests from Concertim UI and to make HTTP/s requests to the OpenStack installation.

## Post-installation configuration

Once Concertim UI has been installed the admin account (see below) will need to configure it with the cloud environment details.  To do so: login as the `admin` user; click on "Cloud environment" in the navigation bar and enter the details for your OpenStack service and the concertim-openstack services.

## User accounts and projects

Concertim UI `v0.2.1` ships with a single admin account by default, namely `admin`, the password is the same as the username.

Each OpenStack project to be managed by Concertim should have a different non-admin Concertim account created and configured for it.  Non-admin accounts can be created from the Concertim UI.  Once the account has been created, a corresponding user and project will be created in OpenStack and linked to the Concertim user account.

Non-admin users can launch clusters by navigating to "Launch cluster" in the navigation bar.  They then select the cluster type they wish to launch; fill in any required details and click "Create". Each cluster will appear in the Concertim UI as a separate rack populated with its devices.

The `admin` user will be able to see and manage all racks and devices, whilst non-admin users will only be able to see racks and devices belonging to their project.
