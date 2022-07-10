# Kubescape CI integration demo 
In this repository I am giving an example of how Kubescape can be integrated into a CI process.

Find the parent project (Kubescape :wink: ) [here](https://github.com/armosec/kubescape)

## What do you want from Kubescape in CI?
Kubescape is a great tool to check your Kubernetes objects for security issues. It works both against live clusters and pre-deployment phases like YAML files or Helm charts.

You can make sure in your CI processes that your code (YAMLs and Helm charts) meets the security policies you want to comply with. Kubescape has the broadest set of controls and frameworks. 

Kubescape can be applied both in push events and pull requests. You can make sure that every code you accept meets requirements.

## What is in this repository
This repository contains a single [YAML file](sources/nginx-deployment.yaml) (in sources directory) which is used as an example Kubernetes manifest. Kubescape supports multiple YAMLs and multiple Helm charts. It finds them according to the command line definition (the positional arguments).

This repository also contains an "exception definition file" called [exceptions.json](.github/assets/kubescape/exceptions.json). It contains all the exceptions the developer wanted to apply to the validation process. See more about exceptions [here](https://github.com/armosec/kubescape/blob/master/examples/exceptions/README.md)

At last, the repository defines a YAML scanning process which can be seen in [build.yaml](.github/workflows/build.yaml), you can see the way Kubescape is invoked.

You should check out the GitHub actions runs of this repository [here](https://github.com/slashben/kubescape-ci-demo/actions)

## Workflow
Kubescape finds potential security problems in you YAML files. You should go over the results and you can:
1. Fix the issue - you can run Kubescape with `--verbose` flag and get assistance of how to solve a given problem
2. Create an exception for the issue - this should happen if the problem is not solvable or not interesting (see the [exceptions.json](.github/assets/kubescape/exceptions.json))

## CI process
Explanation of what is happening in [build.yaml](.github/workflows/build.yaml):
1. Checkout is done
2. Kubescape is installed in the build environment
3. Kubescape is ran on the contents of `sources` directory
  * `-t 0` means that the risk threshold is set to minimum, if the risk is above `0` Kubescape returns a non-zero value and the GitHub action fails
  * `--exceptions .github/assets/kubescape/exceptions.json` exceptions of the scan are take from the [exceptions.json](.github/assets/kubescape/exceptions.json)
  * `-f junit` the output format of Kubescape to be set to JUnit - this is a format the "Publish Test Results" step understands
  * `-o results.xml` the output will be stored in this file

## Things to note
### Exceptions on controls
For the sake of simplicity, this [exceptions.json](.github/assets/kubescape/exceptions.json) contains a single exception applied to all objects and covering all the problematic controls. In general, this is not a good solution, because it is too broad. You might ok with root containers with one Deployment, but others should comply with the "non-root" container requirements. 
Therefore it is strongly suggested to create exceptions on resource and controls together not to be lenient. 

### Threshold is set to the maximum
In the demonstration, we are setting the risk acceptance threshold to it most strict setting (0 risk is accepted). This means every issue either needs to be fixed or an explicit exception needs to be in place. This setting makes sure that Kubescape as a quality gate behaves completely predictable and every issue has to be addressed one way or another.

### Single YAML
This demo contains a single file, however this is just for the sake of simplicity of the example. Of course real life examples are more complicated.









