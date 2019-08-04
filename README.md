# How to harden a new ubuntu server.

Taken and modified from: https://ryaneschinger.com/blog/securing-a-server-with-ansible/

1) Create the vm with digital ocean, aws, azure, etc. Get the ip address
(all of the next steps are done from your local machine):
2) brew install ansible (if on mac)
3) Create a salt (random string) = salt
4) Generate a root password (something strong) and store in your password keeper = root_pass 
5) Use openssl to generate a hashed password with the salt + password
`openssl passwd -salt $salt -1 $root_pass`
6) copy that output and insert into `UBUNTU_COMMON_ROOT_PASSWORD` in five_minutes.yml
7) Generate a user password (something strong) and store in your password keeper = user_pass
8) Use openssl to generate a hashed password with the salt + password 
`openssl passwd -salt $salt -1 $user_pass`
9) copy that output and insert into `UBUNTU_COMMON_DEPLOY_PASSWORD` in five_minutes.yml
10) Insert an email in `UBUNTU_COMMON_LOGWATCH_EMAIL` in five_minutes.yml
11) Insert the ip address of the vm into inv.ini
12) `ansible-playbook five_minutes.yml -i inv.ini -u root`
13) `ssh deploy@IP_ADDRESS`

Disclaimer: I make no claims as to the security veracity of this process. If you look at the link and see what's being done in the ansible five_minutes.yml you are doing basic stuff - changing the root password, creating a non-root user, adding that non-root user to sudo group, making that non-root user able to login via ssh, setting up a basic firewall, configuring non standard ssh port, setting up logwatch, preventing password authentication, preventing root ssh access. There are many many security things that can be hardened beyond that but these are a good start.