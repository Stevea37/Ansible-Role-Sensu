# stevenayers.Sensu

### Ansible role for configuring Sensu.

**WARNING** This playbook only supports RHEL/CentOS at the moment.


## Example configuration
In our configuration, we are using Host and Group variables, which you can find detailed in the [best practise documentation detailed here.](http://docs.ansible.com/ansible/playbooks_best_practices.html#group-and-host-variables)

### Playbook configuration
You will define a very simple role definition inside your playbook, like so:

```
- hosts: all
  remote_user: ansible 
  sudo: yes
  roles:
    - role: sensu
```

### Group Variable configurations
For variables which are consistent across all boxes, define them inside **group_vars/all.yml:**

```
# If you have full access to the internet, you won't need to define the 'gem_repo' or 'package' variable.
# You can find defaults listed in the table below.

gem_repo:
  url: "http://localhost:8081/nexus/content/repositories/gems/"
  use_url: true

package:
  url: 'https://sensu.global.ssl.fastly.net/yum/6/x86_64/sensu-0.29.0-7.el6.x86_64.rpm'
  use_url: true

subscriptions:
 - "base"

plugins:
 - "sensu-plugins-memory-checks"
 - "sensu-plugins-disk-checks"
 - "sensu-plugins-load-checks"

```

You can then overwrite these values at group or host level according to how your inventory is organised. For example:

**inventory:**
```
[webservers]
webserver1.domain.com
10.1.1.10
webserver2
```

**group_vars/webservers.yml:**
```
subscriptions:
 - "base"
 - "web"

plugins:
 - "sensu-plugins-memory-checks"
 - "sensu-plugins-disk-checks"
 - "sensu-plugins-load-checks"
 - "sensu-plugins-http-checks"
```

**host_vars/webserver1.domain.com:**
```
subscriptions:
 - "base"
 - "web"
 - "jenkins"

plugins:
 - "sensu-plugins-memory-checks"
 - "sensu-plugins-disk-checks"
 - "sensu-plugins-load-checks"
 - "sensu-plugins-http-checks"
 - "sensu-plugins-jenkins-checks"
```


## Variables

| Name                  | Default Value                | Description                  |
|-----------------------|------------------------------|------------------------------|
| environment_name      | 'default'                    | Environment the Sensu Server and Client are in, a server and client can only be in one environment at a time. |
| gem_repo.url          | '' (empty string)            | Private gem repo url, specify the repo url here. |
| gem_repo.use_url      | false                        | If using a private gem repo, set this to true, otherwise leave it as false. |
| package.url           | '' (empty string)            | If you are installing the sensu package via a url, perhaps inside your network, specify the url here. |
| package.use_url       | false                        | If you are installing the sensu package via a url, perhaps inside your network, set this to true, otherwise leave it as false. |
| gem_executable        | '/opt/sensu/embedded/bin/gem'| The gem executable path used for installing sensu plugins. |
| rabbitmq_host         | 'localhost'                  | The hostname of your RabbitMQ Instance. |
| rabbitmq_port         | 5671                         | The port of your RabbitMQ Instance, the default port is the SSL port.|
| rabbitmq_vhost        | '/sensu'                     | The virtual host for RabbitMQ |
| rabbitmq_user         | 'sensu'                      | The default user for RabbitMQ |
| rabbitmq_password     | 'randompassword'             | The default password for RabbitMQ. You **MUST** change this password. |
| rabbitmq_use_ssl      | true                         | Specify whether or not you're using SSL for RabbitMQ transport. |
| rabbitmq_ssl_cert_path| '/etc/sensu/ssl/cert.pem'    | Destination for RabbitMQ SSL cert path. |
| rabbitmq_ssl_key_path | '/etc/sensu/ssl/key.pem'     | Destination for RabbitMQ SSL key path. |
| subscriptions         | [] (empty list)              | A list of the subscriptions you want the target node to be enrolled with. We recommend defining this inside group_vars or host_vars. |
| plugins               | [] (empty list)              | A list of the plugins you want installed on the target node. We recommend defining this inside group_vars or host_vars. |
