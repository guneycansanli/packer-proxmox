# Packer for Proxmox with Gitlab-ci
Build a Proxmox VM template with Hashicorp Packer, 

I use [this](https://github.com/ChristianLempa/boilerplates/tree/main/packer/proxmox) repo as a base of my Packer templates and write a blog post about it [here](https://medium.com/@saderi/create-vm-templates-on-proxmox-with-packer-84723b7e3919/).

## Requirements

- Proxmox API token with permissions to create VMs and templates
- Packer 1.7.2 or higher

## How this works
Packer will create a VM from a Proxmox template, and then run a series of scripts to configure the VM. Once the scripts have completed, the VM will be shutdown and converted to a template.

## How to use
You can clone this repo and run the packer commands, or you can use the Gitlab CI/CD pipeline to build the template.

IN gitalb-ci `init` and `validate` jobs automatically run but `build` job is manual.

For variables we have two options, files or environment variables. For files you can create a `variables.json` file and pass it to packer with the `-var-file` option, or you can create a `variables.auto.pkrvars.hcl` file and packer will automatically load it.

For environment variables you can set them in your shell, or you can set them in the Gitlab CI/CD pipeline.

### Variables
| Variable | Description | Required | Default |
| --- | --- | --- | --- |
| `PROXMOX_API_URL` | Proxmox API URL | Yes | |
| `PROXMOX_API_TOKEN_ID` | Proxmox API token id | Yes | |
| `PROXMOX_API_TOKEN_SECRET` | Proxmox API token secret | Yes | |
| `RUNNER_IP_ADDRESS` | IP address of the Gitlab runner | Yes | Only for build |
| `PASSWORD` | Password for user `ubuntu` | Yes | Only for build |

### Username and password
The default username for the VM is `ubuntu`. 

If you run Packer locally you must edit Packer template files and `user-data` files to set password and hashed password. If you are using the Gitlab CI/CD pipeline, just set the plaintext `PASSWORD` variable.

**Note:** You don't need hashed password on CI/CD pipeline, it will be hashed automatically. just set `PASSWORD` variable.

**Note:** You can use `openssl` command to generate hashed password. For example:
```
openssl passwd -6 -salt xyz your_password
```
