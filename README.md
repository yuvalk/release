# OpenShift Release Tooling

TEST This repository holds OpenShift cluster manifests, component build manifests and
CI workflow configuration for OpenShift component repositories for both OKD and
OCP.

## CI Workflow Configuration

To setup a CI workflow for a new repository, use `make new-repo`. See the
[Contributing CI Configuration to the openshift/release Repository](https://docs.ci.openshift.org/docs/how-tos/contributing-openshift-release/)
document for detailed information about how to contribute to this repository.

Configuration files for CI workflows live under [`ci-operator/`](./ci-operator/)
and are split into the following categories:

 - [`ci-operator/config`](./ci-operator/config/) contains configuration for the
   `ci-operator`, detailing builds and tests for component repositories.
 - [`ci-operator/jobs`](./ci-operator/jobs/) contains configuration for `prow`,
   detailing job triggers. In almost all cases, this configuration is
   generated automatically from the `ci-operator` config. For manual edits, see
   [this section](https://docs.ci.openshift.org/docs/how-tos/contributing-openshift-release/#component-ci-configuration)
   of the contribution document and the [upstream configuration document](https://github.com/kubernetes/test-infra/blob/master/prow/README.md#how-to-add-new-jobs). Prefer the `ci-operator` config whenever possible.
 - [`ci-operator/step-registry`](./ci-operator/step-registry/) contains the
   registry of reusable test steps and workflows. See the documentation for
   this content [here](https://steps.ci.openshift.org/help).
 - **[LEGACY]** [`ci-operator/templates`](./ci-operator/templates/) contains black-box test
   workflows for use by the `ci-operator`. The parent directory's
   [README](./ci-operator#end-to-end-tests) documents how to use them. See the
   [template document](https://github.com/openshift/ci-tools/blob/master/TEMPLATES.md)
   for general information on template tests. Templates are legacy and new
   ones should not be added. Multi-stage tests using steps from the shared
   registry should be used instead.

## Cluster and Service Configuration Manifests

### Core Services and Configuration

Manifests for important services (like [OpenShift CI cluster](https://api.ci.openshift.org/)
and the CI service components that run on it) are present in the
[`core-services`](./core-services) directory. The services configured in this
directory are critical for some part of the OpenShift project development
workflow, must meet basic quality criteria and must be deployed to the cluster
automatically by a postsubmit job. See its [README](./core-services/README.md)
for details.

### Additional Services and Configuration

The [`services`](./services) directory contains manifests for services that run
on [OpenShift CI cluster](https://api.ci.openshift.org/) and are not critical
for the OpenShift project development workflow, but want to follow the same
quality criteria and automated check and deployment process that the [core
services](#core-services-and-configuration) do (DPTP intends to support only
these additional services). See its [README](./services/README.md) for details.

### Cluster Configuration Manifests (legacy)

Manifests for cluster provisioning and installation live under [`cluster/`](./cluster/).
The [OpenShift CI cluster](https://api.ci.openshift.org/) is configured with the
manifests under [`cluster/ci/`](./cluster/ci/). (**legacy**: do not add new
services here. Use [`core-services`](./core-services) or
[`services`](./services) instead.)

Clusters that are created by the testing infrastructure for validating OpenShift
are configured with the profiles under [`cluster/test-deploy/`](./cluster/test-deploy/).
For directions on how to set up clusters for development, see the
[README](./cluster/test-deploy/README.md).

### Legacy Service Configuration

Manifests for services that are in development, experimental, legacy or not
critical in some other way are present in the [`projects`](./projects)
directory. Compared to the [core services](#core-services-and-configuration) and
[services](#additional-services-and-configuration) configuration,
these projects do not need to follow any common structure or conventions other
than clear ownership. They must not interfere with the core services in any way.

Additionally, manifests for building container images for component repositories
live under [`projects/`](./projects/). This purpose is deprecated; authors of
components built by manifests in this directory should remove them and ensure
that their component is appropriately built by the `ci-operator` instead.

## Tooling Build Manifests

Manifests for building container images for tools live under [`tools/`](./tools/).
These tools are either useful in managing this repository or are otherwise useful
commonly across component repositories.
