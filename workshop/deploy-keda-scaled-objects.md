# Deploy KEDA Scaled Objects

### Deploy TriggerAuthentication

Before deploying the ScaledObjects, create a `TriggerAuthentication` so that KEDA has access to your Kafka instance. Create it using the `deployments/keda-auth.yaml` yaml file:

```text
oc apply -f deployments/keda-auth.yaml
```

This yaml file uses the `kafka-credentials` secret you created in the previous steps.

### Deploy ScaledObjects

You can now create the `ScaledObject` in `deployments/keda-scaler.yaml`. The `TriggerAuthentication` is referenced in this file.  First, **modify** `bootstrapServers: YOUR_BOOTSTRAP_SERVER` with the ones you have.

{% hint style="info" %}
Your bootstrap server is in your `deployments/kafka-secret.yaml` file.
{% endhint %}

Then create using the oc cli:

```text
oc apply -f deployments/keda-scaler.yaml
```

In the ScaledObject definition, the `lagThreshold: '5'` is the consumer lag that scales the target deployment when it meets this threshold. You can read more about the Kafka scaler [here](https://keda.sh/docs/2.2/scalers/apache-kafka/)

To check the status of your scalers:

```text
oc get scaledobjects

### OUTPUT:
NAME                     SCALETARGETKIND      SCALETARGETNAME   MIN   MAX   TRIGGERS   AUTHENTICATION                       READY   ACTIVE   AGE
courierconsumer-scaler   apps/v1.Deployment   courier           2     6     kafka      keda-trigger-auth-kafka-credential   True    False    23d
kitchenconsumer-scaler   apps/v1.Deployment   kitchen           2     6     kafka      keda-trigger-auth-kafka-credential   True    False    23d
orderconsumer-scaler     apps/v1.Deployment   orders            2     6     kafka      keda-trigger-auth-kafka-credential   True    False    23d
statusconsumer-scaler    apps/v1.Deployment   status            2     6     kafka      keda-trigger-auth-kafka-credential   True    False    23d
```

You should see the **READY** status values are **True**.

