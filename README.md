# ansible testing environment
    
test will be run with help of `ubuntu` as the host for running the ansible commands
and will use some machines being ran on `virtualbox` hypervisor.

## machines

|os|application|ip|username|password|    
|---|---|---|---|---|
|ubuntu|host for vm and ansible|192.168.56.1|senaps||
|centos|websrver|192.168.56.100|root|123|
|ubuntu|websrver|192.168.56.101|senaps|123|

we have defined the machine in the `resources` file, which looks like:

    [web]
    192.168.56.100 ansible_user=root
    192.168.56.101 ansible_user=senaps
    
`install_pkgs.yml` is a file where we tell ansible what to do!

    - hosts: web
      tasks:
      - name: Installing packages centos
        yum:
          name: ['vim', 'git', 'tmux'] 
          state: present
        when: ansible_distribution == "CentOS"
        
`host` specifies what group of machines we want to run these tasks on

`yum` is the name of the package manager we want to use

`tasks` is a list of tasks

`name` is the name of the package we want to install or list of packages to be installed on
the system

`state` is what we want to do with the `names` list. `present` means those packages
should be present/installed on the system.

`when` is a conditional that says this task should be executed if dist is centos.

