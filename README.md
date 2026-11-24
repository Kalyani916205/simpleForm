Hmmm… can't reach this page
localhost refused to connect.

Try:
- Search the web for localhost
- Checking the connection
- Checking the proxy and the firewall
ERR_CONNECTION_REFUSED














Don't worry. Your Kubernetes Deployment and Service were created successfully, but the browser cannot connect to the application yet.

Let's troubleshoot it step by step.

First, one important point: Your Service is using NodePort 30083, but Docker Desktop Kubernetes may require port forwarding to access the application through localhost.
Step 1: Check your Service configuration
Run this command in PowerShell:
kubectl describe service my-app-service


Check the following:
- Selector should be app=my-app.
- TargetPort should be 80.
- NodePort should be 30083.
- Endpoints should show pod IP addresses with port 80.
Step 2: Check Service endpoints
kubectl get endpoints my-app-service


If you see pod IP addresses, the Service is connected to your application pods.
Step 3: Use Port Forwarding
Run:
kubectl port-forward service/my-app-service 8081:80


Expected output:
Forwarding from 127.0.0.1:8081 -> 80
Forwarding from [::1]:8081 -> 80


Port forwarding active
Keep this PowerShell window open.
Step 4: Open your browser
Visit:
http://localhost:8081You should see your application.
Step 5: If it still doesn't work
Run these commands and share the output:
kubectl get pods -l app=my-app
kubectl get deployment my-app
kubectl get endpoints my-app-service
kubectl describe service my-app-service


I'll identify exactly where the problem is.

Note: Port forwarding is a reliable way to test your Kubernetes application locally. It does not mean your Kubernetes deployment has failed.

















PS C:\Users\Banoth Bhadramma\kubernetes1-docker> kubectl get endpoints my-app-service
Warning: v1 Endpoints is deprecated in v1.33+; use discovery.k8s.io/v1 EndpointSlice
NAME             ENDPOINTS                       AGE
my-app-service   10.244.0.11:80,10.244.0.12:80   3m31s
PS C:\Users\Banoth Bha












Good! Your output confirms that Kubernetes Service is correctly connected to both application pods. ✅
my-app-service
10.244.0.11:80
10.244.0.12:80


This means:
- Two application pods are available.
- Both pods are connected to the Service.
- Port 80 is correctly configured.
The deprecation warning is informational; it is not an error.
Next step: Port forwarding
Run this command in PowerShell:
kubectl port-forward service/my-app-service 8081:80


Expected output:
Forwarding from 127.0.0.1:8081 -> 80


Keep this PowerShell window open.
Open your browser
http://localhost:8081You should see your application.
If the page still doesn't open
Open another PowerShell window and run:
kubectl get pods -l app=my-app


kubectl logs deployment/my-app
