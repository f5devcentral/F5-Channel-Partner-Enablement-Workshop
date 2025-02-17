# About NGINX One

The F5 NGINX One Console makes it easy to manage NGINX instances across locations and environments. The console lets you monitor and control your NGINX fleet from one place—you can check configurations, track performance metrics, identify security vulnerabilities, manage SSL certificates, and more.

NGINX One offers the following key benefits:

- Centralized control: Manage all your NGINX instances from a single console.
- Enhanced monitoring and risk detection: Automatically detect critical vulnerabilities (CVEs), verify SSL certificate statuses, and identify security issues in NGINX configurations.
- Performance optimization: Track your NGINX versions and receive recommendations for tuning your configurations for better performance.
- Graphical Metrics Display: Access a dashboard that shows key metrics for your NGINX instances, including instance availability, version distribution, system health, and utilization trends.
- Real-time alerts: Receive alerts about critical issues.

## NGINX One API

You can authenticate API requests in two ways: using an API Token or an API Certificate. Below are examples of how to do this with curl, but you can also use other tools like Postman.


- API Token Authentication: An API token grants a user access to the NGINX One REST API
- API Certificate Authentication: Include the client certificate and password in the request. 

..note: The user’s role determines the permissions associated with the API token.

Here’s how to use an API token to authenticate a request to the F5 Distributed Cloud API. This example request lists tenant namespaces for organization plans:

```
curl https://<tenant>.console.ves.volterra.io/api/web/namespaces -H "Authorization: APIToken <token-value>"
```

Here’s how to use an API Certificate to authenticate a request to the F5 Distributed Cloud API. This example request lists tenant namespaces for organization plans:

```
curl https://<tenant>.console.ves.volterra.io/api/web/namespaces --cert-type P12 --cert <api-creds>:<password>
```

API Reference Guide: https://docs.nginx.com/nginx-one/api/api-reference-guide/

### Lab Environment

### Lab Complete