# velero

[Velero](https://velero.io/) is a cluster backup & restore solution.  I can also leverage restic to backup persistent volumes to S3 storage buckets.

In order to backup and restore a given workload, the follow steps should work.

### Backup

> A backup should already be created by either a scheduled, or manual backup

```bash
# create a backup for all apps
velero backup create manually-backup-1 --from-schedule velero-daily-backup
# create a backup for a single app
velero backup create jackett-test-abc --include-namespaces testing --selector "app.kubernetes.io/instance=jackett-test" --wait
```

### Delete Resources

```bash
# delete the helmrelease
kubectl delete hr jackett-test -n testing

# allow the application to redeployed and create the new resources

# delete the new resources
kubectl delete deployment/jackett-test -n jackett
kubectl delete pvc/jackett-test-config
```

### Restore

- Get `sealed-secrets` up and running
- Encrypt secret for velero minio connection
- Push secret
- Push velero
- Get rook-ceph up and running
- Scale down flux!!
- Restore ONLY the following NS
    media
    home
    network


```bash
velero restore create --from-backup velero-weekly-backup-20210511030004 --include-namespaces media --selector "app.kubernetes.io/instance=plex" --wait
```

Restore a specific PVC
```bash
velero restore create --from-backup test-labels --include-namespaces media --selector "service=sonarr" --include-resources persistentvolumes --namespace-mappings media:testing-grounds --wait --show-labels
```

* This should not interfere with the HelmRelease or require scaling helm-operator
* You don't need to worry about adding labels to the HelmRelease or backing-up the helm secret object
