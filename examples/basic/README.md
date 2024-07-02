# Exemplo básico do ingress canary

Exemplo básico do ingress e deployments canary.

Documentação oficial
[Canary Deployments](https://kubernetes.github.io/ingress-nginx/examples/canary)

## Deploy production

```bash
kubectl apply -f examples/basic/deployments/production.yml
kubectl apply -f examples/basic/services/production.yml
kubectl apply -f examples/basic/ingress-routes/production.yml
```

## Testando antes do canary

```bash
for i in $(seq 1 10); do
  curl -s --resolve echo.prod.mydomain.com:80:$(minikube ip) echo.prod.mydomain.com | grep "Hostname"
done
```

## Deploy canary

Deploy dos recursos canary:

```bash
kubectl apply -f examples/basic/deployments/canary.yml
kubectl apply -f examples/basic/services/canary.yml
kubectl apply -f examples/basic/ingress-routes/canary.yml
```

## Testando depois do canary

```bash
for i in $(seq 1 10); do
  curl -s --resolve echo.prod.mydomain.com:80:$(minikube ip) echo.prod.mydomain.com | grep "Hostname"
done

Hostname: canary-5fdf8c5579-lp4mv
Hostname: canary-5fdf8c5579-lp4mv
Hostname: production-59555fb857-8xzrx
Hostname: canary-5fdf8c5579-lp4mv
Hostname: production-59555fb857-8xzrx
Hostname: production-59555fb857-8xzrx
Hostname: production-59555fb857-8xzrx
Hostname: canary-5fdf8c5579-lp4mv
Hostname: canary-5fdf8c5579-lp4mv
Hostname: production-59555fb857-8xzrx
```
