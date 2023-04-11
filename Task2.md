# Documentation for APIService Module

<br>
The ApiService module provides the ApiService class, which is a subclass of the Service class. It is used for initializing the APIs of a microservice, as well as handling logging and attention required errors.

<br>

## Classes

`ApiService` class is used for **initializing** the APIs of a microservice, **handling logging** and **errors**. It inherits from the `Service` class.

<br>

## Methods

- [constructor](#constructor-method)
- [attention_required](#attention_required-method)
- [remove_attention](#remove_attention)
- [initialize_web](#initialize_web)
- [initialize_zookeeper](#initialize_zookeeper)
- [\_do_zookeeper_adv_data](#do_zookeeper_adv)
- [\_on_zkcontainer_start](#zkcontainer_start)
  <br>

The <span style="font-weight:bold; font-style:italic" id="constructor-method">constructor method</span> initializes the ApiService class.

```
__init__(self, app, service_name="asab.ApiService")
```

It takes two parameters

- `app` - the application object
- `service_name` - the name of the service  
  <br>

It initializes the following properties of the class:

| Properties        | Default value    | Used for storing                |
| :---------------- | :--------------- | :------------------------------ |
| WebContainer      | None             | the web container               |
| ZkContainer       | None             | ZooKeeper container             |
| MetricWebHandler  | None             | Metric web handler              |
| AttentionRequired | empty dictionary | attention required errors found |
| Manifest          | None             | manifest file, if it exists     |
| Changelog         | None             | changelog file, if it exists    |

<br>

The <span style="font-weight:bold; font-style:italic" id="attention_required-method">attention_required method</span> is used for adding an attention required error to the AttentionRequired property of the class:

```
attention_required(self, att: dict, att_id=None)
```

It takes the following parameters:

- `att` - the attention required error as a dictionary
- `att_id` - the ID of the attention required error, if it exists (if it does not, a new ID is generated)
- `att_id` - returns the att_id of the attention required error  
  <br>

The <span style="font-weight:bold; font-style:italic" id="remove_attention">remove_attention method</span> is used for removing an attention required error from the AttentionRequired property of the class:

```
remove_attention(self, att_id)
```

It takes in the `att_id` of the attention required error to be removed.  
<br>

The <span style="font-weight:bold; font-style:italic" id="initialize_web">initialize_web method</span> is used for initializing the web container:

```
initialize_web(self, webcontainer=None)
```

It takes in an optional parameter `webcontainer`, which is the web container to be used. If no `webcontainer` is passed, the default web container is used. It initializes the following properties of the class:
<br>

| Properties       | stores:                              |
| :--------------- | :----------------------------------- |
| WebContainer     | web container                        |
| APILogHandler    | web API loggin handler               |
| format           | logging formatter                    |
| Logging          | the logger                           |
| WebHandler       | the API web handler                  |
| DocWebHandler    | the documentation web handler        |
| MetricWebHandler | the metric web handler, if available |

<br>

The <span style="font-weight:bold; font-style:italic" id="initialize_zookeeper">initialize_zookeeper method</span> is used for initializing the ZooKeeper container.

```
initialize_zookeeper(self, zoocontainer=None)
```

It takes in an optional parameter `zoocontainer`, which is the ZooKeeper container to be used. If no `zoocontainer` is passed, the default ZooKeeper container is used.

It initializes the following properties of the class:  
`Zkontainer` - stores the ZooKeeper container and subscribes to the "ZooKeeperContainer.state/CONNECTED!" event and calls the \_on_zkcontainer_start method.  
<br>

The <span style="font-weight:bold; font-style:italic" id="do_zookeeper_adv"> \_do_zookeeper_adv_data method</span> is used for updating the ZooKeeper container with the data from the AttentionRequired property of the class, if the ZooKeeper container exists and is connected.

```
_do_zookeeper_adv_data(self)
```

The method is creating a dictionary called `adv_data` that contains various metadata about the application:

- `appclass` - the name of the class of the application object(self.App)
- `launchtime`- time the application was launched, converted to ISO 8601 format and appended with a 'Z' to indicate UTC time zone
- `hostname` - hostname of the machine where the application is running
- `servername`- name of the server where the application is running
- `processid`- process ID of the Python process running the application

Once `adv_data` has been created, it can be updated with additional key-value pairs that contain more metadata about the application, dpeending on whether certain contiions are met.  
<br>

The <span style="font-weight:bold; font-style:italic" id="zkcontainer_start">\_on_zkcontainer_start</span> is an event handler for the start of a ZooKeeper container.

```
 _on_zkcontainer_start(self, message_type, container)
```

It takes the following parameters:

- `message_type` - indicates the type of message being handled
- `container` - reference to the ZooKeeper container that triggered the event
