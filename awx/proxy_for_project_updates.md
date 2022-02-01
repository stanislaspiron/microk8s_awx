# Ignore Configure proxy for external resources

1. Go to **Settings >> Jobs**
2. Edit **Extra Environment Variables**
```
{
  "HTTPS_PROXY": "http://10.0.0.1:8080",
  "HTTP_PROXY": "http://10.0.0.1:8080",
  "NO_PROXY": "10.0.*.*,demo.local, localhost,kubernetes.default.svc.cluster.local"
}
```

3. Save settings (no need to restart anything)
