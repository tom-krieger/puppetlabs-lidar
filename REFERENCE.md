# Reference
<!-- DO NOT EDIT: This document was generated by Puppet Strings -->

## Table of Contents

**Classes**

* [`lidar::app_stack`](#lidarapp_stack): Configure a node to run LiDAR
* [`lidar::report_processor`](#lidarreport_processor): Simple class to enable the LiDAR report processor

**Data types**

* [`Lidar::Url`](#lidarurl): 

**Tasks**

* [`get_service_status`](#get_service_status): Get the status of the LiDAR docker-compose services
* [`update_all_images`](#update_all_images): Automates the dance needed to get docker-compose to update the container for every service
* [`use_updated_image`](#use_updated_image): Automates the dance needed to get docker-compose to use an updated image

## Classes

### lidar::app_stack

This class takes care of configuring a node to run LiDAR.

#### Examples

##### Use defalts or configure via Hiera

```puppet
include lidar::app_stack
```

##### Manage the docker group elsewhere

```puppet
realize(Group['docker'])

class { 'lidar::app_stack':
  create_docker_group => false,
  require             => Group['docker'],
}
```

#### Parameters

The following parameters are available in the `lidar::app_stack` class.

##### `analytics`

Data type: `Boolean`

Enable/Disable collection of Analytic Data

Default value: `true`

##### `create_docker_group`

Data type: `Boolean`

Ensure the docker group is present.

Default value: `true`

##### `manage_docker`

Data type: `Boolean`

Install and manage docker as part of app_stack

Default value: `true`

##### `https_port`

Data type: `Integer`

Secure port number to access the LiDAR UI

Default value: 443

##### `compose_version`

Data type: `String[1]`

The version of docker-compose to install

Default value: '1.25.0'

##### `image_prefix`

Data type: `String[1]`

The string that comes before the name of each
container. This can be changed to support private
image repositories such as for internal testing or
air gapped environments.

Default value: 'puppet/lidar-'

##### `lidar_version`

Data type: `String[1]`

The version of the LiDAR containers to use

Default value: '1.0.0-alpha'

##### `log_driver`

Data type: `String[1]`

The log driver Docker will use

Default value: 'journald'

##### `docker_users`

Data type: `Optional[Array[String[1]]]`

Users to be added to the docker group on the system

Default value: `undef`

### lidar::report_processor

Simple class to enable the LiDAR report processor

#### Examples

##### Configuration via Hiera with default port

```puppet
---
lidar::report_processor::lidar_url: 'https://lidar.example.com/in'
lidar::report_processor::pe_console: 'pe-console.example.com'
```

##### Configuration via Hiera with custom port

```puppet
---
lidar::report_processor::lidar_url: 'https://lidar.example.com:8443/in'
lidar::report_processor::pe_console: 'pe-console.example.com'
```

##### Configuration in a manifest with default port

```puppet
# Settings applied to both a master and compilers
class { 'profile::masters_and_compilers':
  class { 'lidar::report_processor':
    lidar_url  => 'https://lidar.example.com/in',
    pe_console => 'pe-console.example.com',
  }
}
```

##### Configuration in a manifest with custom port

```puppet
# Settings applied to both a master and compilers
class { 'profile::masters_and_compilers':
  class { 'lidar::report_processor':
    lidar_url  => 'https://lidar.example.com:8443/in',
    pe_console => 'pe-console.example.com',
  }
}
```

##### Send data to two LiDAR servers

```puppet
---
lidar::report_processor::lidar_url:
  - 'https://lidar-prod.example.com:8443/in'
  - 'https://lidar-staging.example.com:8443/in'
lidar::report_processor::pe_console: 'pe-console.example.com'
```

#### Parameters

The following parameters are available in the `lidar::report_processor` class.

##### `lidar_url`

Data type: `Lidar::Url`

The url to send reports to.

##### `enable_reports`

Data type: `Boolean`

Enable sending reports to LiDAR

Default value: `true`

##### `manage_routes`

Data type: `Boolean`

Enable managing the LiDAR routes file

Default value: `true`

##### `facts_terminus`

Data type: `String[1]`



Default value: 'puppetdb'

##### `facts_cache_terminus`

Data type: `String[1]`



Default value: 'lidar'

##### `reports`

Data type: `String[1]`

A string containg the list of report processors to enable

Default value: 'puppetdb,lidar'

##### `pe_console`

Data type: `Optional[Stdlib::Fqdn]`

The FQDN of your PE Console.

Default value: `undef`

## Data types

### Lidar::Url

The Lidar::Url data type.

Alias of `Variant[Array[Stdlib::HTTPUrl], Stdlib::HTTPUrl]`

## Tasks

### get_service_status

Get the status of the LiDAR docker-compose services

**Supports noop?** false

### update_all_images

Automates the dance needed to get docker-compose to update the container for every service

**Supports noop?** false

### use_updated_image

Automates the dance needed to get docker-compose to use an updated image

**Supports noop?** false

#### Parameters

##### `service`

Data type: `Enum[frontdoor,identity,influxdb,ingest-queue,mongo,query,rabbitmq,ui]`

The service you wish to update

