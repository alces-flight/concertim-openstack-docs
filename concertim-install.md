# Concertim Installation and Configuration

Concertim is a visualisation tool for virtual clusters. It is used to visualise instances and metrics collected for those instances. At a high level it consists of the following parts:

The Concertim UI. A web application that visualises the configured racks, their instances and their reported metrics.
Concertim OpenStack service, an OpenStack adapter for the Concertim UI. It interrogates an OpenStack installation, configures the Concertim UI with the instances it finds; interrogates OpenStack for metrics about those instances and reports them to the Concertim UI.


## Concertim UI installation

There are currently two installation mechanisms for the Concertim UI. It can be built as either a number of Docker images or alternatively as a VM.

In either case, the build will download some assets from S3, so you will need to provide S3 credentials to the build. The credentials need to allow downloading from s3://alces-flight/concertim/packages.

## Docker installation

The most recent version of the Concertim UI is `0.1.3`. To install this version as multiple Docker containers follow the instructions at https://github.com/alces-flight/concertim-ansible-playbook/blob/0.1.3/docker/README.md.

The Docker images can be built on and ran on any machine suitable for running Docker containers. This could be the same machine that runs an OpenStack installation, but it doesn't have to be.

The Docker container `concertim_ui_proxy_1` binds its HTTP and HTTPS ports to the Docker host's `9080` and `9443` ports respectively on interface `127.0.0.1`.  Details on configuring the defaults can be found at https://github.com/alces-flight/concertim-ansible-playbook/blob/0.1.2/docker/README.md#configuration.

The HTTP port redirects all traffic to the HTTPS port, as such access to it is optional.

Any users of the Concertim UI will need to be able to access the HTTPS port of the Docker container `concertim_ui_proxy_1` and optionally the HTTP port.  By default bound to port `9443` on the docker host.

The Concertim OpenStack service will need to be able to access the HTTPS port of the `concertim_ui_proxy_1` container. By default bound to port `9443` on the docker host.


## VM installation

Alternatively, the Concertim UI can be built on a dedicated VM. Instructions for this can be found at https://github.com/alces-flight/concertim-ansible-playbook/blob/0.1.3/ansible/README.md.

The VM could be an OpenStack instance running on the OpenStack service that is to be managed, but it doesn't have to be.

Any users of the Concertim UI will need to be able to access port `443` of the VM, and optionally port `80`.

The Concertim OpenStack service will need to be able to access port `443` of the VM.


### Post-installation configuration

Before the Concertim UI is ready to be used, a number of machine templates need to be created. It is expected that these templates will vary from site to site and will match the OpenStack flavours available at that site. An example script of how to create these templates is available at https://github.com/alces-flight/concertim-ct-visualisation-app/blob/0.1.3/docs/api/examples/create-template.sh

To quickly populate multiple example templates the script https://github.com/alces-flight/concertim-ct-visualisation-app/blob/0.1.3/docs/api/examples/populate-templates.sh can be used.

Details on how to run those example scripts can be found at https://github.com/alces-flight/concertim-ct-visualisation-app/blob/0.1.3/docs/api/examples/README.md A brief example session is reproduced below.

Note that if running either of those scripts you will want to check out the entire example directory.

```
cd /path/to/example/scripts/directory
export CONCERTIM_HOST=<IP address of concertim UI>
export AUTH_TOKEN=$(LOGIN=admin PASSWORD=admin ./get-auth-token.sh)

# Create default example templates.  These may or may not be suitable for your site.
./populate-templates.sh

# Create a template specific to your site
./create-template.sh <NAME> <DESCRIPTION> <U_HEIGHT>
```

## Concertim OpenStack service installation

Instructions for installing and configuring the Concertim OpenStack service is given in its README
https://github.com/alces-flight/concertim-openstack-service/tree/develop

It is intended to be installed as a systemd service. It could be installed on the same machine that is running the OpenStack installation but it doesn't have to.

It needs to be able to make HTTP/s requests to the OpenStack installation that it is to monitor and to the Concertim UI.

## User accounts and projects

Concertim UI `0.1.3` ships with a single admin account by default, namely `admin`, the password is the same as the username.

Each OpenStack project to be managed by Concertim should have a different non-admin Concertim account created and configured for it.  Non-admin accounts can be created from the Concertim UI.  Once the account has been created, its OpenStack project ID can be set via the "Account Details" page in the Concertim UI. Setting a project ID is not available for the admin user.

When an account is configured with a project ID, racks and devices will be automatically populated by Concertim OpenStack service with the instances it finds for that project ID.

The `admin` user will be able to see and manage all racks and devices, whilst non-admin users will only be able to see racks and devices belonging to their project.
