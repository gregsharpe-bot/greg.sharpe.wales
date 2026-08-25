---
title: "Terraform and Grafana Synthetic Monitoring"
date: 2026-08-25T01:48:20Z
tags: ['Terraform', 'Grafana', 'Monitoring', 'Infrastructure', 'Tutorial']
draft: false
---

# Introduction

I've been putting together a small [Terraform module](https://github.com/GregSharpe1/terraform-modules/tree/main/grafana/synthetic-monitoring) for creating Grafana Cloud synthetic monitoring checks. Instead of defining each check separately, the module accepts a map of endpoints and creates the HTTP checks from that. It defaults to HTTP checks using the London probe, which is a good starting point for monitoring a few public endpoints.

One useful part of the module is that it works out a sensible check interval from Grafana Cloud's 100,000 monthly executions. As more endpoints are added, the available executions are divided across them, rounded to the nearest five minutes and kept at a minimum of one minute. This means I can add checks without having to manually recalculate the frequency each time, while still keeping an eye on the allowance being used.

```
module "synthetic_monitoring" {
  source = "./modules/synthetic-monitoring"

  endpoints = {
    "website" = {
      target = "https://greg.sharpe.wales"
    }
  }
}
```
