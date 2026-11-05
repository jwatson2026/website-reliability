\# Service selector mismatch



## Scenario

In my local Kubernetes environment, I deliberately changed a

Service selector to investigate how a configuration mismatch

affects application connectivity.



## Working configuration

* Two Nginx Pods with the label app=website.
* A ClusterIP Service selecting app=website.
* A request through the Service successfully returned the Nginx page.



## Change introduced

I changed the Service selector to app=website-v2.

Kubernetes accepted the configuration, but no Pods matched it.



## Investigation

1. Checked the Service's EndpointSlice and found no endpoints.
2. Checked Pod status and labels.
3. Confirmed both Pods were still Running with app=website.
4. Compared those labels with the Service selector.



\# Service selector mismatch.



## Resolution

Changed the selector back to app=website and applied service.yaml.



During the exercise, I also encountered a YAML indentation error.

I inspected the file, corrected the spacing, and validated it with

a server dry run before applying the change.



## Recovery verification

* Both Pod addresses returned to the EndpointSlice.
* A temporary test Pod requested http://website.
* The request returned HTTP 200.



I did not run an HTTP request during the broken state, so I did

not measure the failed request behavior or outage duration.



## Improvement

A dry run checks whether Kubernetes accepts the configuration,

but does not prove the Service matches the intended Pods.

After Service changes, check endpoints and run an HTTP request.

