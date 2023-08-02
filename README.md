# ieam-edge-assets

- [ieam-edge-assets](#ieam-edge-assets)
  - [Assumptions](#assumptions)
  - [Install IEAM **Cluster** Agents](#install-ieam-cluster-agents)
    - [Pre-Requisites](#pre-requisites)
    - [Setting up Ansible project](#setting-up-ansible-project)
    - [Running IEAM agent install with Ansible](#running-ieam-agent-install-with-ansible)
  - [OpenShift Pipelines for CI/CD Demo](#openshift-pipelines-for-cicd-demo)

## Assumptions
- These instructions have only been tested on **MacOS**

## Install IEAM **Cluster** Agents
### Pre-Requisites
1. IBM Edge Application Manager Hub (`v4.5`)
2. OpenShift Cluster with Storage and [Image Registry is exposed](https://docs.openshift.com/container-platform/4.11/registry/securing-exposing-registry.html)
   * The Edge Cluster environment used for demo is found in [TechZone](https://techzone.ibm.com/collection/production-deployment-guides/environments) using NFS for Storage
3. Ensure the following tools are installed:
   1. [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
   2. [Docker](https://docs.docker.com/engine/install/)
   3. [Horizon CLI](https://www.ibm.com/docs/en/eam/4.5?topic=cli-installing-hzn)
   4. [jq](https://jqlang.github.io/jq/download/)
   5. [oc](https://docs.openshift.com/container-platform/4.11/cli_reference/openshift_cli/getting-started-cli.html)

### Setting up Ansible project
1. Install the following Ansible collections:
   * `ansible-galaxy collection install kubernetes.core`
   * `ansible-galaxy collection install community.general`
2. Install Python module dependencies:
   * `pip3 install -r agent-install/ansible/requirements.txt`
3. Add the following files to the `agent-install/ansible/playbooks/files` directory:
   1. `agent-install.cfg`
      * Example config can be found in `agent-install/ansible/playbooks/files/example-agent-install.cfg`
   2. `agent-install.crt`
   3. `agent-install.sh`
4. Set the environment variables in `agent-install/ansible/vars.yml` accordingly
   * Please refer to the manual [IEAM agent install documentation](https://www.ibm.com/docs/en/eam/4.5?topic=clusters-installing-agent) for more details
     * Either set `K8S_AUTH_USERNAME` with `K8S_AUTH_PASSWORD` OR only `K8S_AUTH_TOKEN`

### Running IEAM agent install with Ansible
* **NOTE:** If you run into any issues with logging into the OpenShift image registry using docker when running the below playbook, restart Docker, and run the playbook again
* Run the Ansible playbook:
    * `ansible-playbook agent-install/ansible/playbooks/edge-cluster-agent-install.yml --extra-vars @agent-install/ansible/vars.yml --ask-become-pass`
      * The password to enter is your computer password. This is to allow certain tasks to run with higher privileges.
* You will see the agent running in namespace set for `AGENT_NAMESPACE` in `vars.yml`

## OpenShift Pipelines for CI/CD Demo
* Please refer to the following [repository](https://github.com/Client-Engineering-Industry-Squad-1/ieam-edge-cluster-demo) to setup this demo in your environment
