## Deploy simple web-services and influxDB to Kubernetes

- Deploy simple web-services, service, and ingress

```bash
  kubectl apply -f simple-api.yaml
```

- Deploy influxDB, service, and ingress

```bash
  kubectl apply -f influxdb.yaml
```

- Check and note minikube IP

```bash
  minikube ip
```

- Check if deployment success, will return `true`

```bash
  http://[MINIKUBE_IP]/myservice/health
  http://[MINIKUBE_IP]/influxdb/health
```

## Configure influxDB and telegraf

- Check and note the influxDB service IP

```bash
  kubectl get svc influxdb
```

- Tunneling through minikube service

```bash
  minikube tunnel
```

- Configure the USERNAME, PASSWORD, ORG and BUCKET for influxDB GUI through `http://[INLFUXDB_IP]:8086`

- Go to Data and choose Telegraf tab to create telegraf configuration on your localmachine
- Add this line on telegraf conf to watch simple web-service and influxDB health

```bash
  [[inputs.http_response]]
    name_suffix = "_myservice"
    urls = ["http://192.168.59.100/myservice/health"]
  [[inputs.http_response]]
    name_suffix = "_influxdb"
    urls = ["http://192.168.59.100/influxdb/health"]
```

- Export influxDB token to local machine

```bash
  export INFLUX_TOKEN=<INFLUX_TOKEN>
```

- Run Telegraf on your local machine

```bash
  telegraf --config http://[INFLUXDB_IP]:8086/api/v2/telegrafs/...
```

- Test your metric on Explore tab and note the Script for later use in Grafana monitoring
  ![create metric](https://i.ibb.co/wZnFbSF/Whats-App-Image-2022-08-09-at-1-46-33-AM.jpg)

## Monitoring with Grafana

- Open Grafana GUI through web browser

```bash
  http://localhost:3000
```

- Configure the datasource from influxDB
  ![Data source conf](https://i.ibb.co/gt7YW25/Whats-App-Image-2022-08-08-at-9-39-53-AM.jpg)

- Create Dashboard with noted script metric from InfluxDB before
  ![Grana Dashboard](https://i.ibb.co/6rGJsBj/b091b7d5-8e66-440e-b5e0-98009a1fca91.jpg)
