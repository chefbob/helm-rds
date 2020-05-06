# helm-rds
A low-dependency tool used to retrieves and inject the RDS Cluster EndPoint based on the cluster name.  This code was based heavily on https://github.com/totango/helm-ssm

## Installation
```bash
$ helm plugin install https://github.com/chefbob/helm-rds
```

## Overview
This plugin provides the ability to dynamically retrieve the RDS Cluster EndPoint by passing in the Cluster Identifier and the Endpoint Type, READER or WRITER.  We will also use the first Endpoint in the list if more than one is returned

We use this plugin in combination with Terraform where Terraform creates a new Database and then our Helm code retrieves the endpoint for use with our application.

During installation or upgrade, the Cluster Endpoint is replaced with the actual value.

Usage:
Simply use helm as you would normally, but add 'rds' before any command,
the plugin will automatically search for values with the pattern:
```
{{rds DBClusterIdentifier WRITER aws-region}}
{{rds DBClusterIdentifier READER aws-region}}
```
and replace them with the Endpoint

Optionally, if you have the same DBClusterIdentifier for different profiles, you can set it like this:
```
{{rds DBClusterIdentifier WRITER aws-region profile}}
{{rds DBClusterIdentifier READER aws-region profile}}
```


>Note: You must have IAM access to read the RDS parameters.

>Note #2: Wrap the template with quotes, otherwise helm will confuse the brackets for json, and will fail rendering.

>Note #3: Currently, helm-rds does not work when the value of the parameter is in the default chart values.

---

## Testing
```
helmv2: $./rds.sh install tests/testchart/ --debug --dry-run -f tests/testchart/values.yaml
helmv3: $./rds.sh install test tests/testchart/ --debug --dry-run -f tests/testchart/values.yaml 

```
