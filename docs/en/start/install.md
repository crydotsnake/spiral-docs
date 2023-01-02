# Installation

## Meet Spiral

Spiral Framework is a High-Performance PHP/Go Full-Stack framework and group of over sixty PSR-compatible components. The Framework execution model is based on hybrid runtime where some services (GRPC, Queue, WebSockets, etc.) are handled by Application Server RoadRunner and the PHP code of your application remains in memory on a permanent basis (anti-memory leak tools included).

## Why Spiral?

There are several reasons why you might consider using the Spiral Framework for your web development projects:

- High-Performance: Due to its design and sophisticated application server, Spiral Framework will execute your code up to 10 times faster than Laravel or Symfony without compromising code quality or compatibility with commonly-used libraries.

- Security: Spiral Framework provides all the tools you need to write secure applications with embedded encryption, CSRF protection, cookie anti-tampering, RBAC authorization, token based authentication, validation, and more.

- Ready to Scale: Scale your application quickly with integrated tools for Queue, GRPC, Event broadcasting and more. The supporting application server includes everything you need to write horizontally-scalable applications.

- PSR compatible: Framework implements most of PSR standards. Enjoy the flexibility of using tools you like without worrying about vendor lock or use Spiral components outside of the framework.

- Unlocked Possibilities: Branch out of single stack programming and easily integrate Spiral Framework with any PHP library or extend functionality using Golang. Enhance development by combining a rich business layer with fast concurrent programming.

- General Purpose and Modular: Framework does not limit your design capabilities, create MVC, CQRS, Event-Driven, CLI apps. Install only the dependencies you need.

# Your First Spiral Project

Before installing Spiral project, you should ensure that your server is configured with the following PHP version and extensions:

* PHP 8.1+, 64bit
* *mb-string* extension (spiral is UTF-8 centric framework)

```bash
composer create-project spiral/installer my-app
```

Once application is installed, you can ensure that it has been configured properly by executing:

```bash
php app.php configure
```

To start application server execute:

```bash
./rr serve
```

On Windows:

```bash
./rr.exe serve
```

The application will be available on `http://localhost:8080`.

> **Note**
> Read more about application server configuration [here](https://roadrunner.dev/docs).

## Available Skeletons

Checks our skeleton builds:

- https://github.com/spiral/app - full-stack build
- https://github.com/spiral/app-cli - minimal CLI build
- https://github.com/spiral/app-grpc - GRPC specific build (no views, HTTP)