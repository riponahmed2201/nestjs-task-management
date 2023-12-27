# Task Management Application

## Application Structure

## AppModule (root)

- TasksModule
    - TasksController
    - TaskEntity
    - TaskService
    - TaskRepository
    - Status ValidationPipe

- AuthModule
    - AuthController
    - UserRepository
    - AuthService
    - JwtStrategy
    - UserEntity

## Objectives: NestJs

- NestJS Modules
- NestJS Controllers
- NestJS Services and Providers
- Controller-to-Services communication
- Validation using NestJS Pipes

## Objectives: Back-end & Architecture

- Develop production-ready REST APIs
- CRUD operations (Create, Read, Update, Delete)
- Error Handling
- Data Transfer Objets (DTO)
- System modularity
- Back-end development best practices
- Configuration Management
- Logging
- Secuirity best practices

## Objectives: Persistence

- Connecting the application to a database
- Working with relational database
- Using TypeORM
- Writing simple and complex queries using QueryBuilder
- Performance when working with a database

## Objectives: Authorization/Authentication

- Signing up, signing in
- Authentication and Authorization
- Protected resources
- Ownership of tasks by users
- Using JWT tokens (JSON Web Token).
- Password hashing, salts and properly storing passwords


## NestJS Module

- Each application has at least one module - the root module. That is the starting point of the application.
- Modules are an effective way to organize components by a closely related set of capabilities( e.g. per feature).
- It is a good practice to have a folder per module, containing the module's componets.
- Modules are singletons, therefore a module can be imported by multiple other modules.

### Defining a module

- A module is defined by annotating a class with the `@Module` decorator.
- The decorator provides metadata that Nest uses to organize the application structure.

## @Module Decorator Properties
 
- `providers: ` Array of providers to be available within the module via dependency injection.
- `controller: ` Array of controllers to be instantiated within the module.
- `exports: ` Array of providers to export to other modules.
- `imports: ` List of modules required by this module. Any exported provider by these modules will now be available in our module via dependency injection.

## NestJs Controllers

- Responsible for handling incomming `requests` and returning `response` to the client.
- Bound to a specific `path` (for example, "/tasks" for the task resource).
- Contain `handlers,` which handle `endpoints` and `request methods` (GET, POST, DELETE etcetera).
- Can take advantage of `dependency injection` to consume providers within the same module.

## Defining a controller

- Controllers are defined by decorating a class with the `@Controller` decorator.
- The decorator accepts a string, which is the `path` to be handled by the controller

```ts
@Controller('/tasks)
export class TasksController {
    // ...
}
```

## Defining a Handler

- Handlers are simply methods within the controller class, decorated with decorators such as` @Get,` `@Post,` `@Delete` etcetera.

```ts

@Controller('/tasks)
export class TasksController {
   @Get()
   getAllTasks(){
    //do stuff

    return ...;
   }

   @Post()
   createTask() {
     //do stuff

    return ...;
   }
}

```

## HTTP request incomming 
- Request route to Controller, handler is called with arguments
    - NestJS will parse the relevant request data and it will be available in the handler.
- Hanler hanldes the request
    - Perform operations such as communication with a service. For example, retrieving an item from the database
- Handler returns response value 
    - Response can be of any type and even an exception.
    - Nest will wrap the returned value as an HTTP response and return it to the client.

- `AuthController` <br>
    /auth

    `signin()`  <br>
    POST /auth/signin

    `signout()`  <br>
    POST /auth/signout

- `TasksController` <br>
    /tasks

    `getAllTasks()`  <br>
    GET /tasks

    `getTaskById()`  <br>
    GET /tasks/:id

    `createTask()`  <br>
    POST /tasks

    `deleteTask()`  <br>
    DELETE /tasks/:id

    `updateTaskStatus()`  <br>
    PATCH /tasks/:id

- `UsersController` <br>
    /tasks

    `getAllUsers()`  <br>
    GET /users

    `createUser()`  <br>
    POST /users

    `deleteUser()`  <br>
    DELETE /users/:id

## NestJS Providers

- Can be injected into constructors if decorated as an `@Injectable`, via `dependency injection.`
- Can be a plain value, a class, sync/async factory etc.
- Providers must be provided to a module for them to be usable.
- Can be exported from a module - and then be available to other modules that import it.

## What is a Service?

- Defined as providers. `Not all providers are services.`
- Common concept within software development and are not exclusive NestJS, JavaScript or back-end development.
- Singleton when wrapped with `@Injectable()` and provided to a module. That means, the same instance will be shared across the application - acting as a single source of truth.
- The main source of business logic. For example, a service will be called from a controller to validate data, create an item in the database and return a response.
- Module 
    - Controller
        - Service A
        - Service B
        - Service C
- Example: 
    ```ts
    import { TasksController } from './tasks.controller';
    import { TasksService } from './tasks.service';
    import { LoggerService } from './shared/logger.service';

    @Module({
        controllers: [
            TasksController
        ],
        providers: [
            TasksService,
            LoggerService
        ]
    })

    export class TasksModule {}
    ```

## Dependency Injection in NestJS
- Any component within the NestJS ecosystem can inject a provider that is decorated with the `@Injectable.`
- We define the dependencies in the constructor of the class. NestJs will take care of the injection for us, and will then be available as a class property.
- Example 
    ```ts
    import { TasksService } from './tasks.service';

    @Controller('/tasks)
    export class TasksController {
        constructor(private tasksService: TasksService ) {}

        @Get()
        async getAllTasks() {
            return await this.tasksService.getAllTasks();
        }
    }

    ```