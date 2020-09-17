[![CircleCI](https://circleci.com/gh/giantswarm/istio-operator.svg?style=shield)](https://circleci.com/gh/giantswarm/istio-operator)

# Istio Operator chart

[Istio Operator](https://github.com/istio/operator) is
an open-source Operator for Kubernetes that manages Istio service meshes.

Giant Swarm offers the community Istio Operator as a Playground App which can be installed, tested, and played with to help manage Istio.


*What is it?*

  It is an operator that handles and abstract all complexity to manage Istio deployments.

*Why did we add it?*
    
  Our customers are interested in testing and running an Istio Service Mesh to benefit from its features. At the same time, helm chart installation from the stable chart is not supported anymore. The new way to install uses this operator (or the command-line tool)

*Who can use it?*

  It has been tested in AWS and Azure, but it should work the same in KVM. Just take into account Istio is deployed with an Ingress Gateway, so in case of KVM it will need new mapped ports.

*How to use it?*
    
  Deploy the operator passing the `values.yaml` that defines an Istio Service Mesh with the exact profile desired.

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

In case you see the problems during the deployment due to lacking `IstioOperator` Custom Resource Definition, please apply it manually 

```bash
> kubectl apply -f https://raw.githubusercontent.com/istio/operator/master/deploy/crds/istio_v1alpha1_istiooperator_crd.yaml
```

## Credit

* https://github.com/istio/operator/

## Security Policy

### Reporting a Vulnerability

Please visit https://www.giantswarm.io/responsible-disclosure for information on
reporting security issues.
