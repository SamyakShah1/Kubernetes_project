# Kubernetes_project
Kubernetes Application Deployment
🚀 Project Overview
This project demonstrates deploying a Flask-based application on a Minikube Kubernetes cluster with various testing scenarios, including scalability, self-healing, rolling updates, and persistent storage.

📁 Project Structure
├── deployment.yaml         # Kubernetes deployment file
├── service.yaml            # Kubernetes service file
├── pvc.yaml                # Persistent Volume Claim
├── Dockerfile              # Docker build file
├── app.py                  # Flask application
├── requirements.txt        # Dependencies for the app
└── README.md               # Project documentation
 Steps Performed
✅ 1. Application Deployment
Built a Docker image for the Flask application and pushed it to Docker Hub.

Deployed the application to Minikube using deployment.yaml.

Exposed the application via NodePort Service.

✅ 2. Application Availability Test
Verified application accessibility using:

sh
Copy
Edit
kubectl get services
minikube service my-app --url
curl http://<EXTERNAL-IP>:<PORT>
✅ 3. Scaling Test
Set up Horizontal Pod Autoscaler (HPA):

sh
Copy
Edit
kubectl autoscale deployment my-app --cpu-percent=50 --min=1 --max=5
Simulated high load using a BusyBox load generator:

sh
Copy
Edit
kubectl run -it --rm load-generator --image=busybox -- /bin/sh -c "while true; do wget -q -O- http://my-app:5000; done"
Verified pod scaling using:

sh
Copy
Edit
kubectl get hpa
kubectl get pods -w
✅ 4. Rolling Update & Rollback Test
Updated the application image and observed a zero-downtime update:

sh
Copy
Edit
kubectl set image deployment/my-app my-app=samyakshah1/my-app:v2
Rolled back to the previous version if needed:

sh
Copy
Edit
kubectl rollout undo deployment/my-app
✅ 5. Pod Failure & Self-Healing Test
Deleted a running pod and checked if it was recreated:

sh
Copy
Edit
kubectl delete pod <POD_NAME>
kubectl get pods -w
✅ 6. Persistent Storage Test
Mounted a Persistent Volume and checked data retention after a pod restart.

✅ 7. Logging Test
Verified application logs:

sh
Copy
Edit
kubectl logs <POD_NAME>
🛑 Stopping the Cluster
To stop everything:

sh
Copy
Edit
kubectl delete deployment my-app
kubectl delete service my-app
minikube stop
minikube delete
📌 Notes
This project was tested locally on Minikube.
