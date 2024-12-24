## 🖥️ Deploy do K3D

1.  Instale o K3D:

    `curl -s https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash`

2.  Crie um cluster:
    
    `k3d cluster create -p 443:443 -p 80:30010` 
    
## 🖥️ Deploy do ArgoCD
    
1.  Crie um namespace:
    
    `kubectl create namespace argocd` 
    
2.  Aplique o manifesto do ArgoCD:

    `kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml` 

3.  Edite o deployment do ArgoCD-server:

	`kubectl edit -n argocd deployments/argocd-server`
    
4.  Adicione a flag **--insecure**:
```
containers:                                                                                                                                          
- args:
  - /usr/local/bin/argocd-server
  - --insecure
```
 5. Crie um Ingress para o ArgoCD:
```
#File: ingress.yaml

apiVersion:  networking.k8s.io/v1  
kind:  Ingress  
metadata:  
	name:  argocd-ingress  
spec:  
	rules:  
	-  http:  
		paths:  
		-  path:  /  
			pathType:  Prefix  
			backend:  
				service:  
					name:  argocd-server  
					port:  
						number:  80  # Argocd-Server service port
```
 6. Aplique o manifesto:

 `kubectl apply -f ingress.yaml -n argocd`
 
## 🖥️ Acessar o ArgoCD
 
Default User: `admin`
Password: Use o comando abaixo:

`kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d`
 
