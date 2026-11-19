# Step 4 (optional): Share Data Registry assets

[← Step 3: Use the API](../03-use-registry-api/README.md) · [Guide index](../README.md)

**Minimum persona:** An **RHOAI user** with project `admin` permissions in the
project containing the assets.

This optional step explores sharing after Steps 2 and 3. The collection and
assets belong to the `data-registry-example` project. You can share the assets
through a person's **project role**, or grant access to **Data Registry assets
only**. Choose based on whether that person also needs access to other resources
in the project. If you followed the optional
[sample users and roles guide](../supplementary/users-and-roles.md), use
`rhoai-user-data-curator` as the person granting access and
`rhoai-user-recipient` as the recipient. Otherwise, use your own RHOAI user
with project `admin` permissions as the grantor and another RHOAI user as the
recipient; replace `rhoai-user-recipient` in the commands below with that
user's username.

## Option 1: Share the whole project

On the RHOAI 3.6 installation used for this guide, Data Registry permissions
are included in the standard OpenShift project roles:

| Project role | Data Registry access inherited | Scope of the grant |
| --- | --- | --- |
| `view` | View collections and assets. | Read access to other project resources allowed by the project role. |
| `admin` | View, create, update, and delete collections and assets. | Administration of other project resources as well. |

For example, if a teammate already has `view` in `data-registry-example`, they
can view its Data Registry assets without another RoleBinding. An RHOAI user
with project `admin` permissions can manage those assets and grant project
access to another RHOAI user by running **one** of these commands:

```bash
# Give the RHOAI user project-wide read access, including Data Registry assets.
oc adm policy add-role-to-user view rhoai-user-recipient -n data-registry-example

# Or give the RHOAI user project-wide administration, including Data Registry
# assets.
oc adm policy add-role-to-user admin rhoai-user-recipient -n data-registry-example
```

For a group, use the matching command:

```bash
oc adm policy add-role-to-group view example-group -n data-registry-example

# Or, for project-wide administration:
oc adm policy add-role-to-group admin example-group -n data-registry-example
```

Project roles affect more than Data Registry, so use this option when the
person needs the corresponding access to the project as a whole.

## Option 2: Share only Data Registry assets

> **Project visibility:** A Data Registry-only RoleBinding grants access to
> registry resources, but might not make the project appear in the dashboard
> selector. The recipient must also be able to discover the project. This guide
> does not yet define a tested least-privilege project-discovery grant. A
> namespace-scoped RoleBinding does not grant access to the cluster-scoped
> Project resource, and a `get`-only rule may not be enough if the selector
> lists projects. Verify the selector using the recipient's account before
> treating this option as complete.

If a teammate needs the catalog but should not receive broad access to other
resources in `data-registry-example`, bind one of the operator-provided Data
Registry ClusterRoles **in that project**:

| Data Registry ClusterRole | Access within `data-registry-example` |
| --- | --- |
| `feast-data-registry-viewer` | View collections and assets. |
| `feast-data-registry-admin` | View, create, update, and delete collections and assets. |

For an RHOAI user, select the level of access and run **one** command:

```bash
# Let the RHOAI user view Data Registry assets in this project.
oc adm policy add-role-to-user feast-data-registry-viewer \
  rhoai-user-recipient -n data-registry-example

# Or let the RHOAI user manage Data Registry assets in this project.
oc adm policy add-role-to-user feast-data-registry-admin \
  rhoai-user-recipient -n data-registry-example
```

For a group, use the matching group command:

```bash
oc adm policy add-role-to-group feast-data-registry-viewer \
  example-group -n data-registry-example

# Or, for people who should manage the registry assets:
oc adm policy add-role-to-group feast-data-registry-admin \
  example-group -n data-registry-example
```

Equivalent RoleBinding examples are provided for a [viewer user](rbac/viewer-user-rolebinding.yaml),
[viewer group](rbac/viewer-group-rolebinding.yaml),
[admin user](rbac/admin-user-rolebinding.yaml), and
[admin group](rbac/admin-group-rolebinding.yaml). Replace the example subject
and project before applying a manifest. The RoleBinding grants the selected
ClusterRole only within its namespace; it does not change the operator-managed
ClusterRole or grant project-wide administration.

These roles cover Data Registry resources **throughout the selected project**.
They do not select individual collections or assets. OpenShift permissions are
additive: a Data Registry RoleBinding cannot remove access that the RHOAI user
already has through a project role or another binding.

## Verify the result

A RHOAI user with project `admin` permissions can inspect the recipient's
effective rules:

```bash
oc auth can-i --list -n data-registry-example --as=rhoai-user-recipient
```

Look for the `dataregistry.opendatahub.io` rules. Then make an actual Data
Registry request as that user, for example by opening **AI hub → Data** if the
project appears in its selector. For registry-only access, check a separate
project resource too:

```bash
oc auth can-i --as=rhoai-user-recipient get pods -n data-registry-example
```

That command should return `no` if the RHOAI user has no other grant to pods in
the project. Some `oc` versions cannot check Data Registry's virtual resources
directly with `oc auth can-i`; the effective rule list and an actual Data
Registry request are more reliable checks.

These grants control catalog access. Access to the underlying S3 objects or
other data sources depends on their own credentials and policies. Review those
separately when sharing an asset or its project.
