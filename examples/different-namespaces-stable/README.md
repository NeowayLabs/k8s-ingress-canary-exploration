# Exemplo básico do ingress canary

Exemplo do uso do ingress canary pelo header `X-Canary: true`
em diferentes namespaces e com um ingress para rotas estáveis.

No 

## Deploy production

```bash
kubectl create namespace production-namespace
kubectl apply -n production-namespace -f examples/different-namespaces/deployments/production.yml
kubectl apply -n production-namespace -f examples/different-namespaces/services/production.yml
kubectl apply -n production-namespace -f examples/different-namespaces/ingress-routes/production.yml
```

## Testando antes do canary

```bash
for i in $(seq 1 10); do
  curl -s --resolve echo.prod.mydomain.com:80:$(minikube ip) echo.prod.mydomain.com | grep "Hostname"
done

Hostname: production-59555fb857-j65w7
Hostname: production-59555fb857-j65w7
Hostname: production-59555fb857-j65w7
Hostname: production-59555fb857-j65w7
Hostname: production-59555fb857-j65w7
Hostname: production-59555fb857-j65w7
Hostname: production-59555fb857-j65w7
Hostname: production-59555fb857-j65w7
Hostname: production-59555fb857-j65w7
Hostname: production-59555fb857-j65w7
```

## Deploy canary

Deploy dos recursos canary:

```bash
kubectl create namespace canary-namespace
kubectl apply -n canary-namespace -f examples/different-namespaces-stable/deployments/canary.yml
kubectl apply -n canary-namespace -f examples/different-namespaces-stable/services/canary.yml
kubectl apply -n canary-namespace -f examples/different-namespaces-stable/ingress-routes/canary.yml
```

## Testando depois do canary

```bash
for i in $(seq 1 10); do
  curl -s --resolve echo.prod.mydomain.com:80:$(minikube ip) echo.prod.mydomain.com -H "X-Canary: true" | grep "Hostname"
done

Hostname: canary-5fdf8c5579-dcf6j
Hostname: canary-5fdf8c5579-dcf6j
Hostname: canary-5fdf8c5579-dcf6j
Hostname: canary-5fdf8c5579-dcf6j
Hostname: canary-5fdf8c5579-dcf6j
Hostname: canary-5fdf8c5579-dcf6j
Hostname: canary-5fdf8c5579-dcf6j
Hostname: canary-5fdf8c5579-dcf6j
Hostname: canary-5fdf8c5579-dcf6j
Hostname: canary-5fdf8c5579-dcf6j
```

## Ingress stable

```bash
kubectl apply -n canary-namespace -f examples/different-namespaces-stable/ingress-routes/stable.yml
```

```bash
for i in $(seq 1 10); do
  curl -s --resolve echo.prod.mydomain.com:80:$(minikube ip) echo.prod.mydomain.com/stable -H "X-Canary: true" | grep "Hostname"
done

Hostname: production-59555fb857-j65w7
Hostname: production-59555fb857-j65w7
Hostname: production-59555fb857-j65w7
Hostname: production-59555fb857-j65w7
Hostname: production-59555fb857-j65w7
Hostname: production-59555fb857-j65w7
Hostname: production-59555fb857-j65w7
Hostname: production-59555fb857-j65w7
Hostname: production-59555fb857-j65w7
Hostname: production-59555fb857-j65w7
```
