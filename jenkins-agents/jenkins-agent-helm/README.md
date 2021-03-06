# Jenkins Helm Agent

This agent extends the base jenkins agent image and adds the helm binary. We can use this agent image in a Jenkins pipeline to perform helm operations in projects.

## Build in OpenShift
```bash
oc process -f ../../.openshift/templates/jenkins-agent-generic-template.yml \
    -p NAME=jenkins-agent-helm \
    -p SOURCE_CONTEXT_DIR=jenkins-agents/jenkins-agent-helm \
    -p DOCKERFILE_PATH=Dockerfile \
    | oc create -n openshift -f -
```
For all params see the list in the `../../.openshift/templates/jenkins-agent-generic-template.yml` or run `oc process --parameters -f ../../.openshift/templates/jenkins-agent-generic-template.yml`.

## Note about ct
`ct` (aka `Chart Testing`) is a tool for testing Helm charts in a monorepo. More information about this tool can be found at the project's [GitHub page](https://github.com/helm/chart-testing).

When using `ct`, you may encounter the following error in a multibranch pipeline job:
```
Error linting charts: Error identifying charts to process: Error running process: exit status 128
```

Follow [this GitHub issue](https://github.com/helm/chart-testing/issues/186#issuecomment-615995590) if you run into this error. This is caused by shallow cloning, which should be disabled to fix this error.
