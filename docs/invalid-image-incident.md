\# Invalid image tag during a deployment update



\## Scenario

I deliberately changed the website image from nginx:stable to

nginx:invalid-version-demo in my local Kubernetes environment.



\## Investigation

The new Pod showed ErrImagePull, followed by ImagePullBackOff.

I inspected its events using kubectl describe pod.



The events reported that the requested image tag was not found.



Both original Pods remained running. A request through the

website Service returned HTTP 200 during the failed update.

The rollout later exceeded its progress deadline.



\## Recovery

I restored nginx:stable in deployment.yaml.



During recovery, I corrected a missing container name and a

YAML indentation error. I inspected the saved file and validated

it with a server dry run before applying it.



\## Verification

\- The Deployment returned to two ready and available Pods.

\- Both Pods were up to date.

\- The failed-image Pod was removed.

\- A request through the Service returned HTTP 200 after recovery.



These were individual request checks, not continuous monitoring.



\## Lessons

Pod events identified the missing image tag.

ImagePullBackOff meant Kubernetes was delaying its next retry.



The failed update did not prevent the tested requests from

succeeding because the original healthy Pods remained available.



\## Improvements

Check image availability before deployment and add an automated

HTTP check after deployment.

