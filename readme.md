[![Build Status](https://cloud.drone.io/api/badges/cuisongliu/drone-kube/status.svg)](https://cloud.drone.io/cuisongliu/drone-kube)

# drone-kube

## config

> config kubeconfig, from env generator /root/.kube/config ,the user controller k8s cluster

## command generator 


```bash
 drone-kube config  --admin xxx  --admin-key xxx --ca xxx  --server xxx
```

### use docker  generator

```bash
 docker run -ti --network=host -e KUBE_SERVER=https://xxx:6443  -e KUBE_CA=xxx  -e KUBE_ADMIN=xxx -e KUBE_ADMIN_KEY=xxx cuisongliu/drone-kube bash 
 drone-kube config
```

| describe | env |
| :--- | :---  |  
| ~/.kube/config  server | SERVER , KUBE_SERVER , PLUGIN_SERVER , PLUGIN_KUBE_SERVER|  
| ~/.kube/config certificate-authority-data | CA, KUBE_CA, PLUGIN_CA, PLUGIN_KUBE_CA|  
| ~/.kube/config client-certificate-data | ADMIN, KUBE_ADMIN, PLUGIN_ADMIN, PLUGIN_KUBE_ADMIN |  
| ~/.kube/config client-key-data | ADMIN_KEY, KUBE_ADMIN_KEY, PLUGIN_ADMIN_KEY, PLUGIN_KUBE_ADMIN_KEY |  

### use drone generator 
1. use no prefix classpath
    ```yaml
    - name: deploy-font
      image: cuisongliu/drone-kube
      settings:
        server:
          from_secret: k8s-server
        ca:
          from_secret: k8s-ca
        admin:
          from_secret: k8s-admin
        admin_key:
          from_secret: k8s-admin-key
      commands:
        - drone-kube config  >> /dev/null
        - kubectl delete -f deploy/deploy.yaml || true
        - sleep 15
        - kubectl create -f deploy/deploy.yaml || true
        
    ```

2. use KUBE prefix classpath

    ```yaml
    - name: deploy-font
      image: cuisongliu/drone-kube
      settings:
        kube_server:
          from_secret: k8s-server
        kube_ca:
          from_secret: k8s-ca
        kube_admin:
          from_secret: k8s-admin
        kube_admin_key:
          from_secret: k8s-admin-key
      commands:
        - drone-kube config  >> /dev/null
        - kubectl delete -f deploy/deploy.yaml || true
        - sleep 15
        - kubectl create -f deploy/deploy.yaml || true
    ```
