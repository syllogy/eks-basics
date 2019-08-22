# backup with velero


## Installation 

https://heptio.github.io/velero/v0.11.0/aws-config.html


## backup entire cluster

```
velero backup create cluster-backup
```

## backup with selector

```
velero backup create nginx-backup --selector app=nginx
```


## Scheduled backup 

```
velero schedule create nginx-daily --schedule="0 1 * * *" --selector app=nginx
```

or

```
velero schedule create nginx-daily --schedule="@daily" --selector app=nginx
```


## restore

```
 velero restore create --from-backup nginx-backup
```


check status
```
velero restore get
```


