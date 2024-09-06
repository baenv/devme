# Development Environment Setup with Devbox

This repository contains configuration files to set up a comprehensive development environment using [Devbox](https://www.jetpack.io/devbox/). Devbox is a powerful tool that allows developers to create reproducible and isolated development environments.

## Overview

The setup includes the following main components:

1. Node.js with Canvas support
2. Elixir
3. PostgreSQL with TimescaleDB extension
4. Redis
5. Apache Kafka and Zookeeper

## Configuration Files

### devbox.json

The `devbox.json` file specifies the packages to be installed in the development environment. It includes:

- Node.js (version 18) with necessary libraries for canvas support (Pango, libpng, Cairo, etc.)
- Elixir (version 1.15.7)
- PostgreSQL with TimescaleDB
- Redis (version 7.2.5)
- Apache Kafka (version 2.13-3.8.0) and Zookeeper (version 3.9.2)

This file ensures that all developers working on the project have the same versions of these tools and libraries, reducing "it works on my machine" issues.

### process-compose.yaml

The `process-compose.yaml` file is used to run services locally without containers. It defines the following processes:

1. **PostgreSQL**: Initializes and starts a PostgreSQL server with TimescaleDB extension.
2. **Redis**: Starts a Redis server.
3. **Zookeeper**: Manages the Zookeeper service for Kafka.
4. **Kafka**: Starts the Kafka broker, depending on Zookeeper.

This file allows developers to easily start all necessary services for local development with a single command.

## Getting Started

1. Install Devbox by following the instructions on the [official Devbox website](https://www.jetpack.io/devbox/docs/installing_devbox/).
2. Clone this repository.
3. Run `devbox shell` to enter the development environment.
4. Use `devbox services up` to start all the services defined in `process-compose.yaml` in the management mode.

Now you have a fully configured development environment with all necessary services running locally!


## Interacting with Services

### PostgreSQL

To interact with PostgreSQL:
1. Create DB
```
createdb --host=localhost --port=5432 --username=postgres --password postgres
```

2. Add timescaledb extension
```
psql --username=postgres --host=localhost --port=5432 --dbname=postgres -c 'CREATE EXTENSION IF NOT EXISTS timescaledb CASCADE;'
```

### Redis CLI

To interact with Redis using the Redis CLI:

1. Open a new terminal window.
2. Enter the Devbox shell by running `devbox shell`.
3. Use the Redis CLI command to connect to your Redis server:

   ```
   redis-cli
   ```

4. You can now run Redis commands directly. For example:

   ```
   SET mykey "Hello"
   GET mykey
   ```

5. To exit the Redis CLI, type `exit` or press Ctrl+C.

### Kafkactl

To interact with Kafka using Kafkactl:

1. Open a new terminal window.
2. Enter the Devbox shell by running `devbox shell`.
3. Use Kafkactl commands to interact with your Kafka cluster. Here are some common operations:

   - List topics:
     ```
     kafkactl get topics
     ```

   - Create a new topic:
     ```
     kafkactl create topic my-topic --partitions 3 --replication-factor 1
     ```

   - Produce messages to a topic:
     ```
     kafkactl produce my-topic --value=test
     ```

   - Consume messages from a topic:
     ```
     kafkactl consume my-topic --from-beginning
     ```

   - Describe a topic:
     ```
     kafkactl describe topic my-topic
     ```

Remember to replace `my-topic` with your actual topic name when using these commands.

For more advanced usage and options, refer to the [Kafkactl documentation](https://github.com/deviceinsight/kafkactl).



## Benefits

- **Consistency**: Ensures all developers use the same versions of tools and libraries.
- **Isolation**: Keeps project dependencies separate from your system-wide installations.
- **Ease of Use**: Simplifies the process of setting up and running multiple services.
- **Reproducibility**: Makes it easy to recreate the development environment on any machine.

Happy coding!
