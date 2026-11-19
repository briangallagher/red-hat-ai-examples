# Getting started with the Data Registry in RHOAI 3.6

The Data Registry is a Technology Preview catalog for data assets in an RHOAI
project. It stores metadata and location/connection references; the data stays
in its source system. This walkthrough enables the registry and shows how to
discover and read an asset from a workbench.

This suggested multi-persona workflow uses an **RHOAI admin** for setup
(Step 1) and an **RHOAI user** with project `admin` for Steps 2–3. One person
can fill both roles, but RHOAI admin status alone does not imply project
`admin`. Step 4 is optional.

> **Optional prerequisite — sample users:** Follow the
> [Create sample users and roles](supplementary/users-and-roles.md) guide for
> separate accounts: `rhoai-admin` (Step 1),
> `rhoai-user-data-curator` (Steps 2–3), and `rhoai-user-recipient` (Step 4).
> Use your own RHOAI accounts if they have the required access.

> **Optional prerequisite — S3 storage:** Use the
> [SeaweedFS evaluation setup](supplementary/s3-compatible-storage.md) for the
> sample object, or use an existing approved S3-compatible service.

> **Technology Preview:** Technology Preview features are not supported with
> Red Hat production service-level agreements (SLAs), might not be functionally
> complete, and are not recommended for production.

## Follow the four steps

| Step | Guide | Outcome | User |
| --- | --- | --- | --- |
| 1 | [Configure registry](01-configure-data-registry/README.md) | Enabled registry and dashboard | RHOAI admin |
| 2 | [Register asset](02-register-data-asset/README.md) | S3 object registered as an asset | RHOAI user (project `admin`) |
| 3 | [Use API](03-use-registry-api/README.md) | Read an asset; register another via API | RHOAI user (project `admin`) |
| 4&nbsp;(optional) | [Share assets](04-share-registry-access/README.md) | Project-wide or registry-only access | RHOAI user (project `admin`) |

The guide targets RHOAI 3.6. Before applying commands, an RHOAI admin should
verify component settings and role names on the cluster. Step 2 needs an
existing project where the RHOAI user has `admin`; Step 3 also needs Workbenches,
an attached S3 connection, and a namespaced registry role for its ServiceAccount.
