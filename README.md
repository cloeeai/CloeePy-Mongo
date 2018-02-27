# CloeePy-Mongo
MongoDB Plugin for CloeePy

Attaches a PyMongo DB connection to CloeePy application context

## Installation

`pip install CloeePy-Mongo`

## Configuration

### Configuration Basics
CloeePy-Mongo configuration must be placed under `CloeePy.Plugins.cloeepy_mongo` in your config file.
The parameters are simply the available PyMongo connection parameters. For more
information on possible configurations please see [Pymongo's MongoClient Documentation](http://api.mongodb.com/python/current/api/pymongo/mongo_client.html#pymongo.mongo_client.MongoClient)

```
CloeePy:
  ...
  Plugins:
    cloeepy_mongo:
      host: localhost
      port: 27017
      username: admin
      password: password
      authSource: admin
      authMechanism: SCRAM-SHA-1
      maxPoolSize: 100
```

### Customize Plugin Namespace

By default, your mongo connection is available on the CloeePy application context as
`app.mongo`. Optionally you can specify a different namespace by which you access
the mongo connection via `pluginNamespace`.

```
...
Plugins:
  cloeepy_mongo:
    pluginNamespace: customMongoNS
    host: localhost
    port: 27017
    username: admin
    password: password
    authSource: admin
    authMechanism: SCRAM-SHA-1
    maxPoolSize: 100

```

Then, you would access your mongo connection on the application context like so:

```
app = CloeePy()
result = app.customMongoNS.admin.command("isMaster")
app.log.info(result)
```

### Optional Environment Variables

It's best practice NOT to store sensitive data, such as database usernames and passwords,
in plain-text configuration files. Thus, CloeePy-Mongo supports configuring your mongo username
and password via environment variables.

You need to set the following:

- Username: `CLOEEPY_MONGO_USERNAME`
- Password: `CLOEEPY_MONGO_PASSWORD`

By doing so, you can omit `username` and `password` in your configuration file.


## Usage
```
import os
from cloeepy import CloeePy

if __name__ == "__main__":
  # Required: set config path as environment variable
  os.environ["CLOEEPY_CONFIG_PATH"] = "./example-config.yml"

  # instantiate application instance
  app = CloeePy()

  # write a log entry to stdout
  app.log.info("Hello World!")

  # Make PyMongo call and log to stdout
  app.log.info(app.mongo.admin.command("isMaster"))
```
