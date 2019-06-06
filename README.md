# ansible testing environment
    
test will be run with help of `ubuntu` as the host for running the ansible commands
and will use some machines being ran on `virtualbox` hypervisor.

## machines

|os|application|ip|username|password|    
|---|---|---|---|---|
|ubuntu|host for vm and ansible|192.168.56.1|senaps||
|centos|websrver|192.168.56.100|root|123|
|centos|dbserver|192.168.56.102|root|123|
|ubuntu|websrver|192.168.56.101|senaps|123|
|ubuntu|dbserver|192.168.56.103|senaps|123|

we have defined the machine in the `resources.yml` file, which looks like:

    ---
    all:
      hosts:
          app_centos:
            ansible_host: 192.168.56.100
            ansible_user: root
          db_centos:
            ansible_host: 192.168.56.102
            ansible_user: root
          app_ubuntu:
            ansible_host: 192.168.56.101
            ansible_user: senaps
          db_ubuntu:
            ansible_host: 192.168.56.103
            ansible_user: senaps
    app:
      hosts:
        app_centos:
        app_ubuntu:
    db:
      hosts:
        db_centos:
        db_ubuntu:

using the syntax above, we are creating some server machines in `all`. this is all the machines that we
have. we could use `all` to install general applications. like updates, `vim` and etc.

then, we have some other names such as `app` and `db` which act as a specific group of machines. we are using
an alias style of calling the servers. we have created the `app_*` and `db_*` machines on the `all` listing, and
are now just calling the name without being forced to call the whole host and user specifications again.
    
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

