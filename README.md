STEP 3

After success:

kubectl get pods
kubectl get svc
STEP 4 (Access)

Try:

http://localhost:30010

If not working (Docker Desktop issue):

kubectl port-forward service/html-demo-service 30010:80
