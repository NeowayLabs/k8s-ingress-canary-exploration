# Exemplo básico do ingress canary

Exemplo do uso do ingress canary pelo header `X-Canary: true`

## Deploy production

```bash
kubectl apply -f examples/by-header/deployments/production.yml
kubectl apply -f examples/by-header/services/production.yml
kubectl apply -f examples/by-header/ingress-routes/production.yml
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
kubectl apply -f examples/by-header/deployments/canary.yml
kubectl apply -f examples/by-header/services/canary.yml
kubectl apply -f examples/by-header/ingress-routes/canary.yml
```

## Testando depois do canary

```bash
for i in $(seq 1 10); do
  curl -s --resolve echo.prod.mydomain.com:80:$(minikube ip) echo.prod.mydomain.com -H "X-Canary: true" | grep "Hostname"
done

Hostname: canary-5fdf8c5579-ghj5z
Hostname: canary-5fdf8c5579-ghj5z
Hostname: canary-5fdf8c5579-ghj5z
Hostname: canary-5fdf8c5579-ghj5z
Hostname: canary-5fdf8c5579-ghj5z
Hostname: canary-5fdf8c5579-ghj5z
Hostname: canary-5fdf8c5579-ghj5z
Hostname: canary-5fdf8c5579-ghj5z
Hostname: canary-5fdf8c5579-ghj5z
Hostname: canary-5fdf8c5579-ghj5z
```
