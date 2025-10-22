# Tomcat Template

## Overview

Generate a servlet-friendly reverse proxy configuration for Apache Tomcat that
handles TLS automation, upstream routing, compression, caching, and HTTP method
allow lists tuned for typical Java web applications.

## Prerequisites

- A Tomcat backend reachable from BunkerWeb (container hostname, service, or
  static IP).
- Access to the BunkerWeb UI or the ability to set environment variables for
  template settings.
- Ensure Tomcat recognises the proxy headers (`proxyName`, `proxyPort`, and
  `scheme`) if you terminate TLS at BunkerWeb.

## Files

- `template.json` – Template definition that guides TLS, reverse proxy, and
  servlet tuning configuration.

## Setup

1. **Import the template**
   - *UI import (recommended)*: open the BunkerWeb `Templates` page, click
     **Create new template**, switch to **Raw** mode, paste the contents of
     `template.json`, and save.
   - *Plugin bundle*: copy `tomcat/` into the BunkerWeb plugin `templates/`
     directory.
2. **Assign the template** to the service that fronts Tomcat (`USE_TEMPLATE=tomcat`
   or pick it from the UI list).
3. **Review TLS options** so `SERVER_NAME` and certificate automation match your
   public hostname.
4. **Point the reverse proxy** at the Tomcat service (for example
   `http://tomcat:8080`) and confirm DNS or networking between containers.
5. **Tune HTTP settings**: adjust `ALLOWED_METHODS` for your application.
6. **Reload BunkerWeb** and verify you can reach your Tomcat application over
   HTTPS through the proxy.

## Customization Tips

- Increase `MAX_CLIENT_SIZE` if your application handles large uploads (such as
  multi-gigabyte WAR deployments).
- Narrow `ALLOWED_METHODS` when you know the servlet endpoints only accept a
  subset of verbs.
- Disable `USE_CLIENT_CACHE` for highly dynamic applications that must bypass
  browser caches.
- Keep `USE_GZIP=yes` for HTML, CSS, and JSON responses; disable it only if the
  upstream handles compression itself.

## Validation

- Run `jq . template.json` to check for JSON syntax errors before importing.
- Exercise a login flow and any file upload endpoints through BunkerWeb to
  ensure body size limits and method restrictions align with your application.
