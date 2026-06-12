![Logo Clever Cloud](./clever-cloud-logo.png)

# Ruby (Sinatra + Puma) redirections example on Clever Cloud
[![Clever Cloud - PaaS](https://img.shields.io/badge/Clever%20Cloud-PaaS-orange)](https://clever-cloud.com)

This is an example of how to deploy a [Ruby](https://www.ruby-lang.org/) web application,
built with the [Sinatra](https://sinatrarb.com/) DSL and served by the
[Puma](https://puma.io/) web server, on Clever Cloud. It demonstrates how to
define routes and several kinds of HTTP redirections.

The application listens on `0.0.0.0:8080`, as required by the Clever Cloud Ruby runtime.

## About the application

The app exposes a few routes, each showing a different behaviour:

| Route   | Behaviour                                                                 |
| ------- | ------------------------------------------------------------------------- |
| `/`     | Returns a plain `Hello, World!`                                           |
| `/json` | Returns a JSON object with the right `Content-Type`                       |
| `/api`  | Visible redirection (HTTP `307`) to [PokeAPI](https://pokeapi.co/)        |
| `/lost` | Transparent (server-side) redirection that proxies [perdu.com](https://perdu.com) |
| *other* | Any other path replies with a `404` HTTP status code                      |

## Technology stack

- [Ruby](https://www.ruby-lang.org/) 3.4
- [Sinatra](https://sinatrarb.com/) 4.2 — web DSL
- [Puma](https://puma.io/) 8.0 — web server
- [HTTParty](https://github.com/jnunemaker/httparty) 0.24 — HTTP client used for the transparent redirection

## Project structure

```
.
├── config.ru          # Rack entry point
├── config/
│   └── puma.rb         # Puma config: binds 0.0.0.0:8080
├── src/
│   └── app.rb          # Sinatra routes and redirections
├── Gemfile             # Dependencies
├── Gemfile.lock        # Locked dependency versions
└── .ruby-version       # Pins the Ruby version (read by Clever Cloud)
```

## Prerequisites

- [Ruby](https://www.ruby-lang.org/en/documentation/installation/) 3.4 (see `.ruby-version`)
- [Bundler](https://bundler.io/)

## Running locally

```bash
bundle install
bundle exec puma config.ru -C config/puma.rb
```

The application will be accessible at http://localhost:8080.

You can then visit:

- http://localhost:8080/ — `Hello, World!`
- http://localhost:8080/json — a JSON object
- http://localhost:8080/api — a visible `307` redirection
- http://localhost:8080/lost — a transparent redirection

## Deploying on Clever Cloud

To deploy this application, you need [Clever Tools](https://github.com/CleverCloud/clever-tools), the Clever Cloud CLI.

```bash
# Step 1: Install and log in
npm install -g clever-tools
clever login

# Step 2: Create the application
clever create --type ruby ruby-multi-redirect-example

# Step 3: Add your domain (optional but recommended)
clever domain add <YOUR_DOMAIN_NAME>

# Step 4: Deploy
clever deploy

# Step 5: Open it in your browser
clever open
```

And you're done! Your application is deploying and will be available on the
`<YOUR_DOMAIN_NAME>` domain.

### Ruby version

Clever Cloud picks the Ruby version from the `.ruby-version` file (pinned to
`3.4` here). You can also override it by setting the `CC_RUBY_VERSION`
environment variable:

```bash
clever env set CC_RUBY_VERSION 3.4
```

## Troubleshooting

If you encounter issues:

1. Check the application logs: `clever logs`
2. Verify all environment variables are correctly set: `clever env`
3. Make sure the application listens on `0.0.0.0:8080`.

## Contributing

Contributions to improve this deployment example are welcome! Please feel free
to submit pull requests or open issues for any enhancements or bug fixes.

## License

This example is provided under the terms of the MIT license.
