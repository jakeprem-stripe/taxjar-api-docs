# TaxJar API Docs

TaxJar API documentation, powered by [Middleman](https://github.com/middleman/middleman).

## Getting Started

Clone the repo and simply run the following command:

```
bundle install
```

This should install the Ruby dependencies you need to get up and going.

## Development

To develop and preview the documentation locally, use the following Middleman command:

```
middleman server
```

This will watch for changes and compile them on the fly.

## Deployment

Commits to the `master` branch are compiled on the fly and deployed to Amazon S3 via Codeship.
