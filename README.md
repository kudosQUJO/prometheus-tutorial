# Grafana + Prometheus + Slack/PagerDuty/Gmail Step-By-Step Guide

- [Guide 1](https://grafana.com/blog/2019/12/04/how-to-explore-prometheus-with-easy-hello-world-projects/): Setup Prometheus and Grafana
- [Guide 2](https://grafana.com/blog/2020/02/25/step-by-step-guide-to-setting-up-prometheus-alertmanager-with-slack-pagerduty-and-gmail/): Setup Alertmanager & integrate Slack, PagerDuty, & Gmail

## Directory Structure

- server: `prometheus` executable
  - run `./prometheus --config.file=prometheus.yml --web.enable-lifecycle` to start server on http://localhost:9090
    - note: the `--web.enable-lifecycle` flag enables you to reload the prometheus configuration w/o restarting the app via `curl -X POST http://localhost:9090/-/reload`
- node_exporter: is a Prometheus exporter that exposes a wide variety of hardware- and kernel-related metrics. This means that we can use Node Exporter to monitor filesystems, disks, CPUs, network statistics (and others) of our own computer system.
  - run `./node_exporter` to start server on http://localhost:9100
- prom_middleware: a simple Express.js web server
  - run `node index.js` to start a web server on http://localhost:9091
- github_exporter: is a Prometheus exporter that exposes git metrics for a github repo.
  - run `docker-compose up` to start a server on http://localhost:9171
- alert_manager: `alertmanager` executable
  - run `./alertmanager --config.file=alertmanager.yml` to run app on http://localhost:9093
    - note: you can reload the alertmanager configuration w/o restarting the app via `curl -X POST http://localhost:9093/-/reload`

## Environment Variables that You'll Need to Set

- **GITHUB_TOKEN**: GitHub API token to scrape metrics using github_exporter.
- **SLACK_API_URL**: Slack Webhook URL that AlertManager can push to.
- **SLACK_CHANNEL**: Slack Channel that AlertManager can post alerts to.
