# 4. Deploy Microservices

### Deploy MongoDB and Redis

Before deploying the microservices you just built, you will need to have a MongoDB and Redis service first. You can deploy them using the provided yaml file:

```bash
oc apply -f deployments/mongo-dev.yaml
oc apply -f deployments/redis-dev.yaml
```

### Deploy Example Food Delivery Microservices

These deployments are meant for development use only as they only provide minimal features and has no persistence.

Now, you can proceed to deploy the microservices of the example food delivery app:

```bash
oc apply -f deployments/frontend.yaml
oc apply -f deployments/apiservice.yaml
oc apply -f deployments/statusservice.yaml
oc apply -f deployments/orderconsumer.yaml
oc apply -f deployments/kitchenconsumer.yaml
oc apply -f deployments/courierconsumer.yaml
oc apply -f deployments/realtimedata.yaml
oc apply -f deployments/podconsumerdata.yaml
```

You can check their status using the command below. 

```bash
oc get pods
```

Wait for the pods to be **Ready** before proceeding to the next step.

