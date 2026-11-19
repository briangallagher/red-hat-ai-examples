# Step 3: Use the Data Registry API

[← Step 2: Register an asset](../02-register-data-asset/README.md) ·
[Optional Step 4: Share access →](../04-share-registry-access/README.md) ·
[Guide index](../README.md)

**Minimum persona:** An **RHOAI user** with project `admin` permissions in the
project selected for this walkthrough.

Use a Jupyter workbench to inspect the Step 2 asset, download its data through
the attached S3 connection, and register a second catalog entry through the
API.

## 1. Check prerequisites

Complete [Step 2](../02-register-data-asset/README.md). In
`data-registry-example`, confirm that the `customer-support` collection,
`customer-support-faq` asset, and `data-registry-storage` connection exist.
The object `examples/customer-support/customer-support-faq.md` must already be
in the bucket named by the asset and connection. Use your Step 2 project name
in the notebook if you created these resources in another project.

### Who can run this step

Use `rhoai-user-data-curator` from the sample-user guide, or your own OpenShift
user account with project `admin` in the same project used for Step 2.
Cluster-admin access is not required. If needed, ask an RHOAI admin to grant
project `admin` before starting the workbench.

At the notebook prompt, provide a current token for that account and leave
`USER_TOKEN` empty. Do not use a cluster-admin or workbench ServiceAccount token,
or paste a token into a notebook cell.

## 2. Create the workbench and attach the connection

In the RHOAI dashboard, open **Projects**, select your Step 2 project, open
**Workbenches**, and select **Create workbench**. Enter these settings:

| Setting | Selection |
| --- | --- |
| Name | `data-registry-api` |
| Workbench image | **Jupyter \| Data Science \| CPU \| Python 3.12**, version **3.6** |
| Hardware profile | An available CPU profile |
| Home storage | Use the default home storage path and a size allowed by your cluster |
| Connections | **Attach existing connections** → `data-registry-storage` |

## 3. Upload and run the notebook

In JupyterLab, upload
[data-registry-api-walkthrough.ipynb](data-registry-api-walkthrough.ipynb)
to the workbench home folder and open it with the Python 3.12 kernel.

If browser upload is unavailable, use the tested CLI fallback below from this
guide's directory in a checkout. Set `PROJECT` to your Step 2 namespace.

<details>
<summary>CLI upload fallback</summary>

```bash
PROJECT=data-registry-example
POD="$(oc get pod -n "$PROJECT" -l notebook-name=data-registry-api \
  -o jsonpath='{.items[0].metadata.name}')"
CONTAINER="$(oc get pod "$POD" -n "$PROJECT" \
  -o jsonpath='{.spec.containers[0].name}')"
oc cp data-registry-api-walkthrough.ipynb \
  "$PROJECT/$POD:/opt/app-root/src/data-registry-api-walkthrough.ipynb" \
  -c "$CONTAINER"
```

</details>

The notebook is self-contained. Follow its explanations and prompts, and run
the cells in order.

## Troubleshooting

| Symptom | What to check |
| --- | --- |
| No Workbenches page or create action | Ask an RHOAI admin to check the Workbenches component and your project access. |
| Missing `AWS_*` setting | Confirm that `data-registry-storage` is attached in the workbench's **Connections** section, then restart it. |
| Registry HTTP 401 with your token | Check that you supplied a current user token and that the notebook sends it to the registry Service. |
| Registry HTTP 403 | Check the RHOAI user's Data Registry role in this project. Step 4 describes grants for RHOAI users and groups. |
| TLS certificate error | Check that the workbench projects the public `service-ca.crt` and that the Service hostname matches its certificate. Keep verification enabled. |
| Registry timeout | Check workbench readiness and network access to the registry Service on port 443. |
| Connection or path differs | Compare the asset detail page with Step 2. The notebook intentionally rejects a different resource. |
| S3 HTTP 403 or timeout | Check the connection, bucket permissions, and network path. SeaweedFS uses its S3 API on port 8333; its Admin port is not an S3 endpoint. |
| SHA-256 mismatch | Inspect the current object before replacing the fixture. Reupload only when this test key is yours. |
| API create returns a conflict | The notebook checks the existing resource's marker and fields, then stops if they differ. Choose new example names rather than overwriting another entry. |

Continue to [optional Step 4](../04-share-registry-access/README.md) for RHOAI
user and group sharing guidance.
