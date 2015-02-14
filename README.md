# ansible-gce-mesos-playbook
Ansible playbook to provision and orchestrates a multinode mesos cluster in google compute

This playbook uses ansible gce plugin to provision a workstation and a number (5) of slave
servers, then runs the ansible-mesos-playbook from the workstation.

Before you get going, create a new google compute project with billing enabled. Next setup
ansible with the gce plugin, and configure your secrets.

For reference, my secrets look like this.

    $:~/dev/ansible-gce-mesos-playbook$ ls -1 ~/.gce-ansible
    gce.ini
    secrets.py

    $:~/dev/ansible-gce-mesos-playbook$ cat ~/.gce-ansible/gce.ini 
    [gce]
    libcloud_secrets = /home/rfuller/.gce-ansible/secrets.py

    $:~/dev/ansible-gce-mesos-playbook$ cat ~/.gce-ansible/secrets.py  | sed -e "s/'.*@/'secret@/"
    GCE_PARAMS = ('secret@developer.gserviceaccount.com', "/home/rfuller/.ssh/mipoc-gce-pkey.pem")
    GCE_KEYWORD_PARAMS = {'project': 'mipoc-1502'}

Once eventually the gce plugin is configured, create the infrastructure like this:

    GCE_INI_PATH=~/.gce-ansible/gce.ini ansible-playbook -i hosts infra.yml


After the servers are provisioned the workstation playbook should do the rest. (NB: I made a local 
copy of gce.py into the cwd for this to work, not sure if that's required. Couldn't get an inventory
folder to work.)

    GCE_INI_PATH=/home/rfuller/.gce-ansible/gce.ini ansible-playbook -i gce.py workstation.yml

The storm cluster is now running. Possibly you might need to open some ports to connect to marathon, etc.

Feedback welcome.

Next I intend to install apache storm into the mesos cluster...

