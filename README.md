This service posts a message to the *stock-trader-messsages* channel on the ibm-cloud@slack.com server on **Slack**.

This service expects a **JSON** object in the http body, containing the following fields: *id*, *owner*, *old*, and *new*.  It returns a **JSON** object containing the message sent (or any error message from the attempt) and the location (**Slack**).

There is another implementation of this service called *notification-twitter*, which sends the message to **Twitter** instead.  If both implmentations of the *Notification* service are installed, you could use **Istio** *routing rules* to determine which gets used, and under what conditions.  An example Istio route rule is in this repository (and another is in the *notification-twitter* repository).

 ### Prerequisites for OCP Deployment
 This project requires three secrets: `jwt`, and `openwhisk`.
 
 ### Build and Deploy to OCP
To build `notification-slack` clone this repo and run:
```
cd templates

oc create -f notification-slack-liberty-projects.yaml

oc create -f notification-slack-liberty-deploy.yaml -n notification-slack-liberty-dev
oc create -f notification-slack-liberty-deploy.yaml -n notification-slack-liberty-stage
oc create -f notification-slack-liberty-deploy.yaml -n notification-slack-liberty-prod

oc new-app notification-slack-liberty-deploy -n notification-slack-liberty-dev
oc new-app notification-slack-liberty-deploy -n notification-slack-liberty-stage
oc new-app notification-slack-liberty-deploy -n notification-slack-liberty-prod

oc create -f notification-slack-liberty-build.yaml -n notification-slack-liberty-build

oc new-app notification-slack-liberty-build -n notification-slack-liberty-build

```

