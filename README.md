# svix-proxy-action

Leverage Svix CLI's [listen](https://github.com/svix/svix-cli#using-the-listen-command) command to create a proxy to test webhooks inside your Github Action. This is useful for integration tests that want to create a webhook on a service (such as Svix) and test that postbacks work!

## Usage

Create the following workflow in your repository:

_./github/workflows/webhook-proxy.yml_

```yml
name: Svix Proxy

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    name: Webhook Integration Tests
    steps:
      - uses: cerebruminc/svix-proxy-action@latest
        id: proxy
        with:
          port: 8080
      - name: Log URL to send webhook postbacks to
        run: |
          echo "Postback URL -> ${{ steps.proxy.outputs.postback_url }}"
```

This will provide a publicly available URL that you can set as your callback url so webhook systems can send their payloads to. In this example, the payloads will be proxied to localhost:8080 inside the action runner.

## Inputs

Inputs are provided using the `with:` section of your workflow YML file.

| key     | Description                         | Required | Default |
| ------- | ----------------------------------- | -------- | ------- |
| port    | Localhost port to proxy traffic for | true     |         |
| logging | Enable proxy logs                   | false    | false   |

`logging` will emit a URL in your action runner that you can visit to see requests sent to the postback URL.

## Outputs

| key          | Description             | Type   | Example                                                 |
| ------------ | ----------------------- | ------ | ------------------------------------------------------- |
| postback_url | Publicly accessible URL | string | https://play.svix.com/in/c_RGeO7pMi8XwtmannGw2a2qpJD26/ |
