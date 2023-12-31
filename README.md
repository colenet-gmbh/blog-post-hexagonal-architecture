# Hexagonal To-Do List application

This is a demo app that was built as an example accompanying a series of [blog posts][Blog] on how to implement a
hexagonal architecture in a typical Java/Spring application.

## Features

The application exposes the following basic features of a To-Do List:

- List all tasks
- Create a new task (consisting of a description and an optional due date)
- Toggle completion state of an existing task

Additionally, it regularly sends reminders (which is a fancy way of saying, that it logs the tasks as warnings to the
console) for due tasks.

## Usage

The application offers an API that exposes an endpoint for each of the aforementioned features:

- `GET http://localhost:8080/tasks` returns all existing tasks
- `POST http://localhost:8080/tasks` creates a new task, expects the following parameters:
    - `description` (mandatory): The display name of the task
    - `dueDate` (optional): Should be in the format `yyyy-MM-dd`
    - Example: `POST http://localhost:8080/tasks?description=Some%20Task&dueDate=2023-07-01`
- `POST http://localhost:8080/tasks/toggle-completion/{id}` toggles the completion state of the task with the specified
  id

## Configuration

There are some properties in the [application.properties][AppProperties] file that you can change (or override
via [environment variables][Env]) to adjust the behaviour of the application:

- `notification.interval`: The interval (in seconds), in which due notifications are sent
- `startup.exampletasks.create`: Set to `true` or `false` - controls, whether example tasks are created on application
  start
- `storage.type`: Either `cache` or `database`
    - In `cache` mode, the data is stored in-memory and lost on application shutdown
    - In `database` mode, the data is persisted to a MongoDB database (see
      the [MongoDB section](#using-a-mongodb-database) for details)
- `spring.data.mongodb.<PROPERTY_NAME>`: Some properties that are used to configure a MongoDB connection. Only used
  in `database` mode

## Using a MongoDB database

You can follow these [instructions][DbReadme] to easily create a new MongoDB via Docker or use an existing MongoDB of
your choice.
In case you use the provided database configuration, just set the property `storage.type=database` and everything should
work as is.
If you decide to use another database configuration, you may also need to adjust the
various `spring.data.mongodb.<PROPERTY_NAME>` properties. See the [configuration section](#configuration) for more
details on the various configuration properties.

## Build and ruin

As this is a standard Spring Boot Maven application, you can build it with the command

```
mvn -B package --file pom.xml
```

and run it via:

```
mvn spring-boot:run
```

[AppProperties]: src/main/resources/application.properties

[DbReadme]: database/README.md

[//]: # (TODO: Add links to blog posts when they are published)

[Blog]: https://www.colenet.de/blog/

[Env]: https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#features.external-config.typesafe-configuration-properties.relaxed-binding.environment-variables
