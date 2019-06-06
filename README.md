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
    
we use the test `ping` module here

    ansible -i resources web -m ping
    
`-i` is specifing a custom file named `resources` as where the inventory is. inventory
is a place where we place our ip's and servers to manage remotly.

`web` is telling ansible cli to do this command on the machines of the `web` group.
this is a variable and should be the name of the group we want to do the action on

`-m ping` is telling the ansible to execute the ping module. more on than later!

we are now using variables. in current example, we have a `centos` with `root` and an `ubuntu`
machine with `senaps` userame. the point is, when we use

    ansible -i resources web -m ping -u root
    
the `ping` for ubuntu machine will fail since it doesn't have `root` as it's username
we have moved the username to the `resource` file specifing it with `ansible_username` and
then have removed the `-u` directive from the cli.
as the result, ansible will successfully ping both machines with their own username.

we will run ansible with following command now:

    ansible -i resources web -m ping
