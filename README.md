# Ansible-Role-Sensu

## Ansible role for configuring Sensu.

This playbook only supports RHEL/CentOS at the moment.

## Variables

# Ansible-Role-Sensu

## Ansible role for configuring Sensu.

This playbook only supports RHEL/CentOS at the moment.

## Variables

| Name                  | Default Value                | Description                  |
|-----------------------|------------------------------|------------------------------|
| environment_name      | 'default'                    | This specifies the environment a Sensu Server and client are in, a server and client can only server one environment at a time. |
| gem_repo.url          | '' (empty string)            | If you are using a private gem repo, perhaps inside your network, specify the repo url here. |
| gem_repo.use_url      | false                        | If you are using a private gem repo, perhaps inside your network, set this to true, otherwise leave it as false. |
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

