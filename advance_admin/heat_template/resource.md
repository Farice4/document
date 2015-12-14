# 模板资源

栈模板中的资源以一个标识的形式存在，不同的标识代表不同的资源类型。一种资源类型通常代表 OpenStack 中的一种虚拟资源，如
OS::Nova::Server 这个代表虚拟机资源。

### 在线模板资源类型标识列表

栈模板中支持的资源类型列表在 heat
开发文档有一个汇总：[资源列表](http://docs.openstack.org/developer/heat/template_guide/openstack.html)

在其中可以找到每个资源类型标识的详细信息，但该列表很新，有一些列出的资源在我们的
环境中可能尚不支持。

### 使用命令行获取资源类型标识列表

每个部署了 heat 的 OpenStack 环境都可以使用命令行查询其运行中的版本
所支持的资源类型列表。

* 获取资源类型标识列表

  > ```
  > heat resource-type-list
  > ```

* 获取资源类型标识详情

  > ```
  > heat resource-type-show <Resource Type>
  > ```

* 生成一种资源类型的示例模板

  示例模板供制作编排系统模板时参考，并非直接使用。

  > ```
  > heat resource-type-template <Resource Type>
  > ```

### 示例

* 以编排系统可伸缩资源类型 OS::Heat::AutoScalingGroup 为例，获取资源类型详情

```
# heat resource-type-show OS::Heat::AutoScalingGroup

{
  "attributes": {
    "outputs": {
      "description": "A map of resource names to the specified attribute of each individual resource."
    }, 
    "outputs_list": {
      "description": "A list of the specified attribute of each individual resource."
    }
  }, 
  "properties": {
    "resource": {
      "type": "map", 
      "required": true, 
      "update_allowed": true, 
      "description": "Resource definition for the resources in the group, in HOT format. The value of this property is the definition of a resource just as if it had been declared in the template itself.", 
      "immutable": false
    }, 
    "min_size": {
      "description": "Minimum number of resources in the group.", 
      "required": true, 
      "update_allowed": true, 
      "type": "integer", 
      "immutable": false, 
      "constraints": [
        {
          "range": {
            "min": 0
          }
        }
      ]
    }, 
    "desired_capacity": {
      "type": "integer", 
      "required": false, 
      "update_allowed": true, 
      "description": "Desired initial number of resources.", 
      "immutable": false
    }, 
    "cooldown": {
      "type": "integer", 
      "required": false, 
      "update_allowed": true, 
      "description": "Cooldown period, in seconds.", 
      "immutable": false
    }, 
    "rolling_updates": {
      "description": "Policy for rolling updates for this scaling group.", 
      "required": false, 
      "update_allowed": true, 
      "type": "map", 
      "immutable": false, 
      "schema": {
        "min_in_service": {
          "description": "The minimum number of resources in service while rolling updates are being executed.", 
          "default": 0, 
          "required": false, 
          "update_allowed": false, 
          "type": "number", 
          "immutable": false, 
          "constraints": [
            {
              "range": {
                "min": 0
              }
            }
          ]
        }, 
        "pause_time": {
          "description": "The number of seconds to wait between batches of updates.", 
          "default": 0, 
          "required": false, 
          "update_allowed": false, 
          "type": "number", 
          "immutable": false, 
          "constraints": [
            {
              "range": {
                "min": 0
              }
            }
          ]
        }, 
        "max_batch_size": {
          "description": "The maximum number of resources to replace at once.", 
          "default": 1, 
          "required": false, 
          "update_allowed": false, 
          "type": "number", 
          "immutable": false, 
          "constraints": [
            {
              "range": {
                "min": 0
              }
            }
          ]
        }
      }
    }, 
    "max_size": {
      "description": "Maximum number of resources in the group.", 
      "required": true, 
      "update_allowed": true, 
      "type": "integer", 
      "immutable": false, 
      "constraints": [
        {
          "range": {
            "min": 0
          }
        }
      ]
    }
  }, 
  "resource_type": "OS::Heat::AutoScalingGroup"
}
```

* 以虚拟机资源类型 OS::Nova::Server 为例，生成示例模板


```
# heat resource-type-template OS::Nova::Server

HeatTemplateFormatVersion: '2012-12-12'
Outputs:
  accessIPv4: {Description: The manually assigned alternative public IPv4 address
      of the server., Value: '{"Fn::GetAtt": ["Server", "accessIPv4"]}'}
  accessIPv6: {Description: The manually assigned alternative public IPv6 address
      of the server., Value: '{"Fn::GetAtt": ["Server", "accessIPv6"]}'}
  addresses: {Description: A dict of all network addresses with corresponding port_id.,
    Value: '{"Fn::GetAtt": ["Server", "addresses"]}'}
  first_address: {Description: 'Convenience attribute to fetch the first assigned
      network address, or an empty string if nothing has been assigned at this time.
      Result may not be predictable if the server has addresses from more than one
      network.', Value: '{"Fn::GetAtt": ["Server", "first_address"]}'}
  instance_name: {Description: AWS compatible instance name., Value: '{"Fn::GetAtt":
      ["Server", "instance_name"]}'}
  name: {Description: Name of the server., Value: '{"Fn::GetAtt": ["Server", "name"]}'}
  networks: {Description: 'A dict of assigned network addresses of the form: {"public":
      [ip1, ip2...], "private": [ip3, ip4]}.', Value: '{"Fn::GetAtt": ["Server", "networks"]}'}
  show: {Description: A dict of all server details as returned by the API., Value: '{"Fn::GetAtt":
      ["Server", "show"]}'}
Parameters:
  admin_pass: {Description: The administrator password for the server., Type: String}
  admin_user: {Description: 'Name of the administrative user to use on the server.
      This property will be removed from Juno in favor of the default cloud-init user
      set up for each image (e.g. "ubuntu" for Ubuntu 12.04+, "fedora" for Fedora
      19+ and "cloud-user" for CentOS/RHEL 6.5).', Type: String}
  availability_zone: {Description: Name of the availability zone for server placement.,
    Type: String}
  block_device_mapping: {Description: Block device mappings for this server., Type: CommaDelimitedList}
  config_drive:
    AllowedValues: ['True', 'true', 'False', 'false']
    Description: If True, enable config drive on the server.
    Type: Boolean
  diskConfig:
    AllowedValues: [AUTO, MANUAL]
    Description: Control how the disk is partitioned when the server is created.
    Type: String
  flavor: {Description: The ID or name of the flavor to boot onto., Type: String}
  flavor_update_policy:
    AllowedValues: [RESIZE, REPLACE]
    Default: RESIZE
    Description: Policy on how to apply a flavor update; either by requesting a server
      resize or by replacing the entire server.
    Type: String
  image: {Description: The ID or name of the image to boot with., Type: String}
  image_update_policy:
    AllowedValues: [REBUILD, REPLACE, REBUILD_PRESERVE_EPHEMERAL]
    Default: REPLACE
    Description: Policy on how to apply an image-id update; either by requesting a
      server rebuild or by replacing the entire server
    Type: String
  key_name: {Description: Name of keypair to inject into the server., Type: String}
  metadata: {Description: Arbitrary key/value metadata to store for this server. Both
      keys and values must be 255 characters or less.  Non-string values will be serialized
      to JSON (and the serialized string must be 255 characters or less)., Type: Json}
  name: {Description: Server name., Type: String}
  networks: {Description: 'An ordered list of nics to be added to this server, with
      information about connected networks, fixed ips, port etc.', Type: CommaDelimitedList}
  personality:
    Default: {}
    Description: A map of files to create/overwrite on the server upon boot. Keys
      are file names and values are the file contents.
    Type: Json
  reservation_id: {Description: A UUID for the set of servers being requested., Type: String}
  scheduler_hints: {Description: Arbitrary key-value pairs specified by the client
      to help boot a server., Type: Json}
  security_groups:
    Default: []
    Description: List of security group names or IDs. Cannot be used if neutron ports
      are associated with this server; assign security groups to the ports instead.
    Type: CommaDelimitedList
  software_config_transport:
    AllowedValues: [POLL_SERVER_CFN, POLL_SERVER_HEAT, POLL_TEMP_URL]
    Default: POLL_SERVER_CFN
    Description: How the server should receive the metadata required for software
      configuration. POLL_SERVER_CFN will allow calls to the cfn API action DescribeStackResource
      authenticated with the provided keypair. POLL_SERVER_HEAT will allow calls to
      the Heat API resource-show using the provided keystone credentials. POLL_TEMP_URL
      will create and populate a Swift TempURL with metadata for polling.
    Type: String
  user_data: {Default: '', Description: User data script to be executed by cloud-init.,
    Type: String}
  user_data_format:
    AllowedValues: [HEAT_CFNTOOLS, RAW, SOFTWARE_CONFIG]
    Default: HEAT_CFNTOOLS
    Description: How the user_data should be formatted for the server. For HEAT_CFNTOOLS,
      the user_data is bundled as part of the heat-cfntools cloud-init boot configuration
      data. For RAW the user_data is passed to Nova unmodified. For SOFTWARE_CONFIG
      user_data is bundled as part of the software config data, and metadata is derived
      from any associated SoftwareDeployment resources.
    Type: String
Resources:
  Server:
    Properties:
      admin_pass: {Ref: admin_pass}
      admin_user: {Ref: admin_user}
      availability_zone: {Ref: availability_zone}
      block_device_mapping:
        Fn::Split:
        - ','
        - {Ref: block_device_mapping}
      config_drive: {Ref: config_drive}
      diskConfig: {Ref: diskConfig}
      flavor: {Ref: flavor}
      flavor_update_policy: {Ref: flavor_update_policy}
      image: {Ref: image}
      image_update_policy: {Ref: image_update_policy}
      key_name: {Ref: key_name}
      metadata: {Ref: metadata}
      name: {Ref: name}
      networks:
        Fn::Split:
        - ','
        - {Ref: networks}
      personality: {Ref: personality}
      reservation_id: {Ref: reservation_id}
      scheduler_hints: {Ref: scheduler_hints}
      security_groups:
        Fn::Split:
        - ','
        - {Ref: security_groups}
      software_config_transport: {Ref: software_config_transport}
      user_data: {Ref: user_data}
      user_data_format: {Ref: user_data_format}
    Type: OS::Nova::Server
```
