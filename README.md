# Sentinel

Replays your agent's conversations on every pull request and shows what changed: which scenarios
pass, which are flaky, and which started failing.

> This repository is published automatically from the Sentinel monorepo. Don't edit `dist/` or
> open pull requests against it.

## Use it

Add two repository secrets, `SENTINEL_TOKEN` (create one in your project's settings in Sentinel) and
`AGENT_ENDPOINT` (the URL of your agent's chat endpoint), then add this workflow:

```yaml
on:
  pull_request:
    types: [opened, synchronize, reopened]
  push:
    branches: [main] # produces the baseline
permissions:
  contents: read
  pull-requests: write
jobs:
  sentinel:
    runs-on: ubuntu-latest
    timeout-minutes: 15
    steps:
      - uses: OnyxOcelot/sentinel-action@v0
        with:
          sentinel-token: ${{ secrets.SENTINEL_TOKEN }}
          agent-endpoint: ${{ secrets.AGENT_ENDPOINT }}
```

To check your setup without running anything, add `mode: check`.

## Inputs and outputs

See `action.yml`. Both secrets are masked in the log, and your endpoint's address is never sent to
Sentinel.
