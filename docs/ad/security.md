---
layout: default
title: Anomaly Detection Security
nav_order: 10
parent: Anomaly Detection
has_children: false
---

# Anomaly detection security

You can use the security plugin with anomaly detection to limit non-admin users to specific actions. For example, you might want some users to only be able to create, update, or delete detectors, while others to only view detectors.

All anomaly detection indices are protected as system indices. Only a super admin user or an admin user with a TLS certificate can access system indices. For more information, see [System indices](../../security/configuration/system-indices/).


Security for anomaly detection works the same as [security for alerting](../../alerting/security/).

## Basic permissions

As an admin user, you can use the security plugin to assign specific permissions to users based on which APIs they need access to. For a list of supported APIs, see [Anomaly Detection API](../api/).

The security plugin has two built-in roles that cover most anomaly detection use cases: `anomaly_full_access` and `anomaly_read_access`. For descriptions of each, see [Predefined roles](../../security/access-control/users-roles/#predefined-roles).

If these roles don't meet your needs, mix and match individual anomaly detection [permissions](../../security/access-control/permissions/) to suit your use case. Each action corresponds to an operation in the REST API. For example, the `cluster:admin/opendistro/ad/detector/delete` permission lets you delete detectors.

## (Advanced) Limit access by backend role

Use backend roles to configure fine-grained access to individual detectors based on roles. For example, users of different departments in an organization can view detectors owned by their own department.

First, make sure that your users have the appropriate [backend roles](../../security/access-control/). Backend roles usually come from an [LDAP server](../../security/configuration/ldap/) or [SAML provider](../../security/configuration/saml/), but if you use the internal user database, you can use the REST API to [add them manually](../../security/access-control/api/#create-user).

Next, enable the following setting:

```json
PUT _cluster/settings
{
  "transient": {
    "opendistro.anomaly_detection.filter_by_backend_roles": "true"
  }
}
```

Now when users view anomaly detection resources in Kibana (or make REST API calls), they only see detectors created by users who share at least one backend role.
For example, consider two users: `alice` and `bob`.

`alice` has an analyst backend role:

```json
PUT _opendistro/_security/api/internalusers/alice
{
  "password": "alice",
  "backend_roles": [
    "analyst"
  ],
  "attributes": {}
}
```

`bob` has a human-resources backend role:

```json
PUT _opendistro/_security/api/internalusers/bob
{
  "password": "bob",
  "backend_roles": [
    "human-resources"
  ],
  "attributes": {}
}
```

Both `alice` and `bob` have full access to anomaly detection:

```json
PUT _opendistro/_security/api/rolesmapping/anomaly_full_access
{
  "backend_roles": [],
  "hosts": [],
  "users": [
    "alice",
    "bob"
  ]
}
```

Because they have different backend roles, `alice` and `bob` cannot view each other's detectors and its results.
