### Common Issues
1. Pods not starting

  ```
   kubectl describe pod <pod-name> -n user-app
   kubectl logs <pod-name> -n user-app
```

2. Ingress not working

```
   kubectl get ingress -n user-app
   kubectl logs -n ingress-nginx -l app.kubernetes.io/component=controller
```

3. TLS certificate errors
Verify mkcert CA is installed
```
mkcert -CAROOT
```

Reinstall if needed
```
mkcert -uninstall
mkcert -install
```

4. Can't access https://jideka.com.ng

```
cat /etc/hosts | grep jideka.com.ng # Check /etc/hosts
```

- Get Minikube IP
```
minikube ip
```
- Ensure they match
