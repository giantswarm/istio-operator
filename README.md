[![CircleCI](https://circleci.com/gh/giantswarm/istio-operator.svg?style=shield)](https://circleci.com/gh/giantswarm/istio-operator)

# Istio Operator chart

[Istio Operator](https://github.com/istio/operator) is
an open-source Operator for Kubernetes that manages Istio service meshes.

Giant Swarm offers the community Istio Operator as Managed App to help manage Istio in tenant clusters.

## Configuration

Istio Operator relies on a Custom Resource `IstioOperator` to define a spec that helps the users to manage Service Mesh deployments.

In the `values.yaml` the user can define the service mesh instance definition to tell the operator how to build the Istio Mesh. Istio offers [different profiles](https://istio.io/latest/docs/setup/additional-setup/config-profiles/) to select to define which components are going to be installed and what default configuration is defined.

As an example, you can define a new mesh with profile `demo` like:

```yaml
meshes:
  - |
    apiVersion: install.istio.io/v1alpha1
    kind: IstioOperator
    metadata:
      namespace: istio-system
      name: example-istio-controlplane
    spec:
      profile: demo
```

This will provision all Istio components in the targeted cluster. To see all configuration options check this [doc pace](https://istio.io/latest/docs/reference/config/istio.operator.v1alpha1/#IstioOperatorSpec).

## Compatibility

Tested on Giant Swarm release 11.x.x on `AWS` and `Azure`.

## Development

In order to upgrade the Operator, checkout latest version on [official repository](https://github.com/istio/istio/tree/master/operator), the manifest are in the `deploy` folder (though they should no change much). In the [release page](https://github.com/istio/istio/releases) you download the latest version as reference.

In case there is a new image published, go to [retagger](https://github.com/giantswarm/retagger/pull/465/files) and edit the `images.yaml` to point to the latest version allowing to pull the images from Giant Swarm registries. After update the default version on `values.yaml` to contain the latest tagged version.

## Known Issues

Currently the Istio deployment is limited to `istio-system` for security reasons. Let us know if it is an inconvenient for you.

## Credit

* https://github.com/istio/operator/

## Security Policy

### Reporting a Vulnerability

Please visit https://www.giantswarm.io/responsible-disclosure for information on
reporting security issues.
