# Troubleshooting

I Add in this section solutions I found when troubleshooting deployments issues.

## unable to delete awx
When trying to remove awx, if the command does not stop after several minutes, stop the command wirg CTRL-C, then use following command:

```
kubectl patch awx tower -p '{"metadata":{"finalizers":[]}}' --type=merge
```

You can then retry awx deletion.
