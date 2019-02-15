# Development with Devspaces

### Devspaces 

Manage your Devspaces https://www.devspaces.io/.

Read up-to-date documentation about cli installation and operation in https://www.devspaces.io/devspaces/help.

Here follows the main commands used in Devspaces cli. 

|action   |Description                                                                                   |
|---------|----------------------------------------------------------------------------------------------|
|`devspaces --help`                    |Check the available command names.                               |
|`devspaces create [options]`          |Creates a DevSpace using your local DevSpaces configuration file |
|`devspaces start <devSpace>`          |Starts the DevSpace named \[devSpace\]                           |
|`devspaces bind <devSpace>`           |Syncs the DevSpace with the current directory                    |
|`devspaces info <devSpace> [options]` |Displays configuration info about the DevSpace.                  |

Use `devspaces --help` to know about updated commands.

#### Development flow

You should have Devspaces cli services started and logged to develop with Devspaces.
The following commands should be issued from **project directory**.

1 - Create Devspaces

```bash
$ cd devspaces/docker
$ devspaces create
$ cd ../../

```

2 - Start containers

```bash
devspaces start jenkins
```

3 - Start containers synchronization

```bash
devspaces bind jenkins
```

4 - Grab some container info

```bash
devspaces info jenkins
```

Retrieve published DNS and endpoints using this command

5 - Connect to development container

```bash
devspaces exec jenkins
```

6 - Build Jenkins

```bash
mvn clean package -pl war -am -DskipTests -Dfindbugs.skip
```

7 - Run Jenkins

```bash
cd war/target/
nohup java -jar jenkins.war > $LOGFILE 2>&1
```

Access application URLs:

Using information retrieved in step 4, access the following URL's:

* Application (bound to port 8080): 
    * `http://jenkins.<devspaces-user>.devspaces.io:<published-ports>/`
    
8 - Run Tests

```bash 
mvn package -P light-test
```

### Docker Script Manager (CLI)

Currently, we have these command available to work using local docker compose.

```bash
devspaces/docker-cli.sh <command>
```

|action    |Description                                                               |
|----------|--------------------------------------------------------------------------|
|`build`   |Builds images                                                             |                                      
|`deploy`  |Deploy Docker compose containers                                          |
|`undeploy`|Undeploy Docker compose containers                                        |
|`start`   |Starts Docker compose containers                                          |
|`stop`    |Stops Docker compose containers                                           |
|`exec`    |Get into the container                                                    |

#### Development flow

1 - Build and Run `docker-compose` locally.

```bash
devspaces/docker-cli.sh build
devspaces/docker-cli.sh deploy
devspaces/docker-cli.sh start
```

2 - Get into container

```bash
devspaces/docker-cli.sh exec
```

3 - Build Jenkins

```bash
mvn clean package -pl war -am -DskipTests -Dfindbugs.skip
```

4 - Run Jenkins

```bash 
cd war/target/
nohup java -jar jenkins.war > $LOGFILE 2>&1
```

Access application URLs:

* Application: 
    * http://localhost:8080

7 - Run Tests

```bash
mvn package -P light-test
```
