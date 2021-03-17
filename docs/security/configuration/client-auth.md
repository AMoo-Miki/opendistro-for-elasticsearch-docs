---
layout: default
title: Client certificate authentication
parent: Configuration
grand_parent: Security
nav_order: 50
---

# Client certificate authentication

After obtaining your own certificates either from a certificate authority (CA) or by [generating your own certificates using OpenSSL](../generate-certificates), you can start configuring Elasticsearch to authenticate a user using a client certificate.

Client certificate authentication offers more security advantages than just using basic authentication (username and password). Because client certificate authentication requires both a client certificate and its private key, which are often in the user's possession, it is less vulnerable to brute force attacks in which malicious individuals try to guess a user's password.

Another benefit of client certificate authentication is you can use it along with basic authentication, providing two layers of security.

## Enabling client certificate authentication

To enable client certificate authentication, you must first set `clientauth_mode` in `elasticsearch.yml` to either `OPTIONAL` or `REQUIRE`:

```yml
opendistro_security.ssl.http.clientauth_mode: OPTIONAL
```

Next, enable client certificate authentication in the `client_auth_domain` section of `config.yml`.

```yml
clientcert_auth_domain:
  description: "Authenticate via SSL client certificates"
  http_enabled: true
  transport_enabled: true
  order: 1
  http_authenticator:
    type: clientcert
    config:
      username_attribute: cn #optional, if omitted DN becomes username
    challenge: false
  authentication_backend:
    type: noop
```

## Assigning roles to your common name

You can now assign your certificate's common name (CN) to a role. For this step, you must know your certificate's CN and the role you want to assign to. To get a list of all predefined roles in Elasticsearch, refer to our [list of predefined roles](../../access-control/users-roles#predefined-roles). If you want to first create a role, refer to [how to create a role](../../access-control/users-roles#create-users), and then map your certificate's CN to that role.

After deciding which role you want to map your certificate's CN to, you can use [Kibana](../../access-control/users-roles#map-users-to-roles), [`roles_mapping.yml`](../yaml/#roles_mappingyml), or the [REST API](../../access-control/api/#create-role-mapping) to map your certificate's CN to the role. The following example uses the `REST API` to map the common name `CLIENT1` to the role `readall`.

#### Sample request

```json
PUT _opendistro/_security/api/rolesmapping/readall
{
  "backend_roles" : ["sample_role" ],
  "hosts" : [ "example.host.com" ],
  "users" : [ "CLIENT1" ]
}
```

#### Sample response

```json
{
  "status": "OK",
  "message": "'readall' updated."
}
```

After mapping a role to your client certificate's CN, you're ready to connect to your cluster using those credentials.

The code example below uses the Python `requests` library to connect to a local Elasticsearch cluster and sends a GET request to the `movies` index.

```python
import requests
import json
base_url = 'https://localhost:9200/'
headers = {
  'Content-Type': 'application/json'
}
cert_file_path = "/full/path/to/client-cert.pem"
key_file_path = "/full/path/to/client-cert-key.pem"

# Send the request.
path = 'movies/_doc/3'
url = base_url + path
response = requests.get(url, cert = (cert_file_path, key_file_path), verify=False)
print(response.text)
```
