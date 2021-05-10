# 7. Exposing the Application

### Create Route for the frontend

To access the application, you're going to need to expose the frontend and the backend microservices that it will access.

Expose the frontend using the `oc` cli:

```text
oc expose svc/example-food
```

### Get the hostname of the frontend route

Before exposing the backend service, get the hostname of the frontend and use that same hostname for your backend. To get the hostname of your frontend:

```text
oc get route example-food -o jsonpath='{.spec.host}'

example-food-food-delivery.***.appdomain.cloud
```

 Then modify `deployments/routes.yaml` file to use your hostname. Replace the instances of _HOSTNAME_ in `host: HOSTNAME` with yours. Example below:

**deployments/routes.yaml** file:

```yaml
...
...
metadata:
  name: apiservice-path-creatorder
spec:
  host: example-food-food-delivery.***.appdomain.cloud
  path: "/createOrder"
...
```

Then apply the yaml file.

```text
oc apply -f deployments/routes.yaml
```

The routes maps the URL paths to its intended target service. You can verify your routes using: `oc get routes`

```text
oc get routes

### OUTPUT
NAME                   HOST/PORT         PATH           SERVICES              PORT   TERMINATION   WILDCARD
example-food           example-*.cloud                  example-food          8090                 None
path-createorder       example-*.cloud   /createOrder   apiservice            8080                 None
path-podconsumerdata   example-*.cloud   /consumers     podconsumerdata       8080                 None
path-realtimedata      example-*.cloud   /events        realtimedata          8080                 None
path-restaurants       example-*.cloud   /restaurants   apiservice            8080                 None
path-status            example-*.cloud   /status        apiservice            8080                 None
path-user              example-*.cloud   /user          apiservice            8080                 None
```

### Test one of the APIs

You can now execute one of the APIs using the terminal. You should get a similar response. Replace the hostname in the command below:

```text
curl -X POST -H "Content-Type: application/json" -d @restaurants.json http://YOUR_HOSTNAME/restaurants

### OUTPUT
{"requestId":"e2bdbb26-b8ba-406a-acbe-b10e2cfd599b","payloadSent":{"restaurants":[...]}}
```

This should populate the restaurants list in the frontend's mobile simulator.

You can now access the simulator in your browser with the hostname you got from above. \(ex. example-food-food-delivery.\*\*\*.appdomain.cloud\). You can now proceed to the next step.

