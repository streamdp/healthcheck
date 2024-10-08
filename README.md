## Web service healthcheck workflow
[![healthcheck](https://github.com/streamdp/healthcheck/actions/workflows/healthcheck.yml/badge.svg?branch=main)](https://github.com/streamdp/healthcheck/actions/workflows/healthcheck.yml)

This branch implements a GitHub workflow to check the health status of a web service. The purpose of this workflow is to 
continuously monitor the */healthz* endpoint of the service, ensuring that the service is operational and responds with a
status indicating that all is well. It will help developers and operations teams by providing real-time feedback about 
the availability and health of the web service.
### Workflow Key Features:
* The workflow will ping the /healthz endpoint of the web service at regular intervals (5 minutes by default).
* It will check for a **200 OK** response, indicating that the web service is healthy.
* If the status check fails, it will log the failure and notify the team using GitHub notification service.
```shell
jobs:
  check-site:
    strategy:
      matrix:
        url: [ "https://ccd.run.place", "https://ip-info.run.place", "https://weather.line.pm"]
    runs-on: ubuntu-24.04
    steps:
      - name: Check availability of ${{ matrix.url }}
        run: curl -sLIf -m 15 -o /dev/null --retry 5 --retry-all-errors "${{ matrix.url }}/healthz" || exit 1
```
