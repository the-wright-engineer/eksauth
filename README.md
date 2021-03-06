# EKS Auth
This is a simple python module that generates a token compatible with Heptio IAM Authenticator.

Using this module you can authenticate against an EKS cluster from a python script without the need of the heptio authenticator binary

## Usage
```python
from eksauth import auth
from kubernetes import client, config

# Get EKS auth token
eks = auth.EKSAuth('TestBed')
token = eks.get_token()

# Load kubernetes configuration
config.load_kube_config()
configuration = client.Configuration()
configuration.api_key['authorization'] = token
configuration.api_key_prefix['authorization'] = 'Bearer'


api = client.ApiClient(configuration)
v1 = client.CoreV1Api(api)

ret = v1.list_pod_for_all_namespaces(watch=False)

for i in ret.items:
    print("%s\t%s\t%s" % (i.status.pod_ip, i.metadata.namespace, i.metadata.name))

```

## TODO
* Add Docstrings
* Add Error Handling
* Create Tests
* Create package