# Getting started with the Data Registry in RHOAI 3.6

The Data Registry is a Technology Preview catalog for describing data assets
in an RHOAI project. It stores metadata and location/connection references;
the data itself remains in the source system. This customer walkthrough takes
you from an enabled registry to an asset you can discover and read from a
workbench.

This is a suggested multi-persona workflow: an **RHOAI admin** configures the
service in Step 1, while an **RHOAI user** with project `admin` permissions
registers and uses assets in Steps 2 and 3. One person can fill both personas,
but RHOAI admin status alone does not imply project `admin` permissions. The
minimum for Steps 2–4 is an RHOAI user with project `admin`; Step 4 is optional.

> **Optional prerequisite — sample users:** To follow this workflow with
> separate accounts, see [Create sample users and roles](supplementary/users-and-roles.md)
> before Step 1. It uses `rhoai-admin-dave` for Step 1,
> `rhoai-user-builder` for Steps 2–3, and `rhoai-user-recipient` in Step 4.
> Skip it if your existing accounts already have the required access.

> **Technology Preview:** Technology Preview features are not supported with
> Red Hat production service-level agreements (SLAs), might not be functionally
> complete, and are not recommended for production.

## Follow the four steps

| Step | Guide | You will finish with | Minimum persona |
| --- | --- | --- | --- |
| 1 | [Configure the Data Registry](01-configure-data-registry/README.md) | A running registry and dashboard entry. | RHOAI admin |
| 2 | [Register a Data Asset](02-register-data-asset/README.md) | A synthetic document in S3-compatible storage and a discoverable catalog record. | RHOAI user with project `admin` permissions |
| 3 | [Use the Data Registry API](03-use-registry-api/README.md) | A notebook that reads metadata, downloads the document, and registers an API-created asset. | RHOAI user with project `admin` permissions |
| 4 (optional) | [Share Data Registry assets](04-share-registry-access/README.md) | Choose project-wide access or grant an RHOAI user or group access to registry assets only. | RHOAI user with project `admin` permissions |

The example uses `data-registry-example` as the RHOAI project and an
S3-compatible [SeaweedFS evaluation setup](supplementary/s3-compatible-storage.md)
for its sample document. You can use an existing approved S3 service instead.
SeaweedFS is not required for the Data Registry service itself.

The guide is written for RHOAI 3.6. An RHOAI admin should verify component
settings and generated role names against the installed cluster before applying
them. For Step 2, the target project must exist and the RHOAI user must have
project `admin` permissions. The notebook needs an enabled Workbenches
component, an attached S3 connection, and a namespaced registry role for its
ServiceAccount.
