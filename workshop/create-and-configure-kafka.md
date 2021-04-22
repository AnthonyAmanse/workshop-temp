# Create and Configure Kafka Cluster

### Create a basic Kafka cluster for development

Create a basic Kafka cluster for development. Once the cluster is ready, you should be able to create a Topic in the next step.

![](../.gitbook/assets/image%20%287%29.png)

### Create a Topic

Head over to the **Topics** tab. ****Create a topic named **orders** with 6 partitions.

![](../.gitbook/assets/image%20%285%29.png)

### Create API Key

Create an API Key in the **Clients** tab. Take note of the **Key** and **Secret**.

![](../.gitbook/assets/image%20%284%29.png)

### Create a Secret in OpenShift

In the `deployments/kafka-secret.yaml` file, replace the **SASL\_USERNAME** and **SASL\_PASSWORD** with the **Key** and **Secret** respectively. Replace the BOOTSRAP\_SERVERS with the value in `bootstap.servers` you got from above. You can also find this in the **Cluster Settings** tab.

```text
...
  BOOTSTRAP_SERVERS: 'pkc-***.us-east4.gcp.confluent.cloud:9092'
  SECURITY_PROTOCOL: 'SASL_SSL'
  SASL_MECHANISMS: 'PLAIN'
  SASL_USERNAME: '****'
  SASL_PASSWORD: '***'
...
```

Then create the secret in your OpenShift cluster so your microservices can access the Kafka service.

```text
oc apply -f deployments/kafka-secret.yaml
```

You can now proceed to the next step.

