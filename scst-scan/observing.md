# Observing and troubleshooting

This section shows how to observe the scan job and get logs.

## <a id="watching-in-flight-jobs"></a>Watching in-flight jobs
The scan will run inside the job, which creates a Pod. Both the job and Pod will be cleaned up automatically after completion. You can set a watch on the job and Pod before applying a new scan to observe the job deployment.

```
watch kubectl get scantemplates,scanpolicies,sourcescans,imagescans,pods,jobs
```

## <a id="troubleshooting"></a>Troubleshooting
If you run into any problems or face non-expected behavior, you can always address the logs to get more feedback.

```
kubectl -n scan-link-system logs -f deployment/scan-link-controller-manager -c manager
```

### <a id="missing-target-image-pull-secret"></a>Missing target image pull secret

Scanning an image from a private registry requires an image pull secret to exist in the Scan CR's namespace and be referenced as `grype.targetImagePullSecret` in `tap-values.yml`. See [Installing part II: Profiles](../install.md#-install-a-tanzu-application-platform-profile) for more information.

If a private image scan is triggered and the secret is not configured, the scan job will fail with the error as follows:

```
Job.batch "scan-${app}-${id}" is invalid: [spec<!-- |specifications| is preferred. -->.template.spec<!-- |specifications| is preferred. -->.volumes[2].secret.secretName: Required value, spec<!-- |specifications| is preferred. -->.template.spec<!-- |specifications| is preferred. -->.containers[0].volumeMounts[2].name: Not found: "registry-cred"]
```
