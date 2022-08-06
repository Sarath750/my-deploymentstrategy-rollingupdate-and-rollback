# my-deploymentstrategy-rollingupdate-and-rollback

Normally if we want to update our application with zero downtime then we have different strategies.
1. Rolling update and Rollback
2. Blue green deployment
3. Canary deployment

Rollingupdate-Rollback
----------------------

to check total deployment revisions
-----------------------------------
kubectl rollout history deployment springboot-deployment

Check rollout history for revision "1" (our application is running with the image of sarath750/springboot-hello:v1)
-------------------------------------------------------------------------------------------------------------------
kubectl rollout history deployment springboot-deployment --revision=1


kubectl get pods --watch (deplicate the terminal to see changes)


Now i want to change the deployment to newer image sarath750/springboot-hello:v2

Upgrade new image using below command
-------------------------------------
kubectl set image deployment springboot-deployment springboot-deployment=sarath750/springboot-hello:v2

now our deployment took place with same image and new tag(v2). till the new pods gets created with the newer image, the older pods won't be terminating. So, now our deployment happened with zero downtime.

now we can see the new output of "Greetings from Springboot123..!!!" from olderone "Greetings from Springboot..!!!"



kubectl rollout history deployment springboot-deployment
(2 revisions will be there)
1
2
kubectl rollout history deployment springboot-deployment --revision=2
(our new deployment will be running with new image sarath750/springboot-hello:v2)


Rollout to previous version using below command
-----------------------------------------------
If our newer deployment is not working fine then again we need to rollback to older deployment using below command.

kubectl rollout undo deployment springboot-deployment --to-revision=1

kubectl rollout history deployment springboot-deployment
2
3


kubectl rollout history deployment springboot-deployment --revision=3
sarath750/springboot-hello:v1
(rolled back to older version)

now we can see the new output of "Greetings from Springboot..!!!" from olderone "Greetings from Springboot123..!!!"
