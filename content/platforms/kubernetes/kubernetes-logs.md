---
Title: Redis Enterprise Kubernetes Logs
description: 
weight: 30
alwaysopen: false
categories: ["Platforms"]
aliases: /rs/concepts/kubernetes/redis-labs-kubernetes-logs
         /rs/concepts/logs/
---

## Logs
Each redis-enterprise container stores its logs under `/var/opt/redislabs/log`.
When using persistent storage this path is automatically mounted to the
`redis-enterprise-storage` volume.
This volume can easily be accessed by a sidecar, i.e. a container residing on the same pod.

For example, in the REC (Redis Enterprise Cluster) spec you can add a sidecar container, such as a busybox, and mount the logs to there:
```yaml
sideContainersSpec:
  - name: busybox
    image: busybox
    args:
      - /bin/sh
      - -c
      - while true; do echo "hello"; sleep 1; done

    volumeMounts:
    - name: redis-enterprise-storage
      mountPath: /home/logs
      subPath: logs
```

Now the logs can be accessed from in the side card. For example by running

```kubectl exec -it -c busybox <pod-name> tail home/logs/supervisord.log```

The sidecar container is user determined and can be used to format, process and logs and share logs in the desired format and protocol.