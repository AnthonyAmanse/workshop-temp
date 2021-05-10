# 5. Installing KEDA

###  Use OperatorHub to install KEDA

On your OperatorHub of your OpenShift console, locate and install KEDA operator to namespace `keda`. The operator will create the namespace for you if it doesn't exist. It may take a few minutes for it to completely install.

![KEDA in OperatorHub](../.gitbook/assets/image%20%282%29.png)

### Create the KEDA Controller

Create a `KedaController` named `keda` in the namespace `keda` using the KEDA operator with the OpenShift console.

![KedaController](../.gitbook/assets/image%20%283%29.png)

You can verify the installation using the command below. You should get a similar output.

```bash
oc get pods -n keda

### OUTPUT
NAME                                     READY   STATUS    RESTARTS   AGE
keda-metrics-apiserver-87dbd8f8b-zrj72   1/1     Running   0          25d
keda-olm-operator-59c5cbbddb-2f5r5       1/1     Running   0          25d
keda-operator-68d68b797-84qvb            1/1     Running   0          25d
```

You can now deploy other KEDA resources.

