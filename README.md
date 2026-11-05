# Website Reliability

A Kubernetes website project focused on deployment, Service connectivity, and troubleshooting.

## Environment

- Docker Desktop Kubernetes on Windows
- Nginx Deployment with two Pods
- ClusterIP Service
- PowerShell, kubectl, and Git

This is an independent project using simulated incidents.

## How it works

The Deployment maintains two Nginx Pods labeled app=website.
The Service selects Pods with that label and provides an internal address for website requests.

## Service connectivity incident

I deliberately changed the Service selector to app=website-v2.
Kubernetes accepted the configuration, but the Service had no endpoints.

I checked the Pods and confirmed both were still running.
Comparing their labels with the Service selector revealed the mismatch.

I restored the selector to app=website, confirmed both endpoint addresses returned, and tested a request through the Service. It returned HTTP 200.

[Read the incident report](docs/service-selector-incident.md)

## Key lesson

A configuration can pass validation and still cause a connectivity problem.
Checking Pod status, matching labels, endpoint addresses, and an actual HTTP request provided evidence for the diagnosis and recovery.

## Project files

- deployment.yaml: Defines the two-Pod Nginx Deployment.
- service.yaml: Defines the internal website Service.
- docs/service-selector-incident.md: Records the investigation and recovery.

## Limitations

Both Pods run on one local Kubernetes node, so they do not protect against losing that node.

The project does not yet include health probes, automated monitoring, or a deployment pipeline.
