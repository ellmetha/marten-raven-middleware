# Marten Raven Middleware

**Marten Raven Middleware** provides a [Raven.cr](https://github.com/Sija/raven.cr) integration that allows you to get automatic error reporting for the [Marten web framework](https://github.com/martenframework/marten) by using a dedicated [middleware](https://martenframework.com/docs/handlers-and-http/middlewares).

## Installation

Simply add the following entry to your project's `shard.yml`:

```yaml
dependencies:
  marten_raven_middleware:
    github: martenframework/marten-raven-middleware
```

## Configuration

First, add the following requirement to your project's `src/project.cr` file:

```crystal
require "marten_raven_middleware"
```

You should then ensure that Raven.cr is [properly configured](https://github.com/Sija/raven.cr#usage). In a Marten project, you'll likely create a `config/initializers/raven.cr` initializer where you require and configure the Raven client for your project. For example:

```crystal
# config/initializers/raven.cr

require "raven"

Raven.configure do |config|
  config.dsn = "your_dsn"
end
```

To get automatic error reporting for your project, you then need to add the `Raven::Marten::Middleware` middleware class to the [`middleware`](https://martenframework.com/docs/development/reference/settings#middleware) Marten setting. Ideally, this middleware should be placed near the beginning of the `middleware` array to ensure that it can catch any errors raised by the following middlewares in the stack. For example:

```crystal
# config/settings/base.cr

Marten.configure do |config|
  config.middleware = [
    Raven::Marten::Middleware,
    # Other middlewares...
    Marten::Middleware::GZip,
    Marten::Middleware::XFrameOptions,
    Marten::Middleware::StrictTransportSecurity,
  ]
end
```

## Authors

Morgan Aubert ([@ellmetha](https://github.com/ellmetha)) and 
[contributors](https://github.com/ellmetha/marten-raven-middleware/contributors).

## License

MIT. See ``LICENSE`` for more details.
