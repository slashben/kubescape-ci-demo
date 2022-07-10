# Kubescape CI integration demo 
In this repository I am giving an example of how Kubescape can be integrated into a CI process.

Find the parent project (Kubescape :wink: ) [here](https://github.com/armosec/kubescape)

# What do you want from Kubescape in CI?
Kubescape is a great tool to check your Kubernetes objects for security issues. It works both against live clusters and pre-deployment phases like YAML files or Helm charts.

You can make sure in your CI processes that your code (YAMLs and Helm charts) meets the security policies you want to comply with. Kubescape has the broadest set of controls and frameworks. 

Kubescape can be applied both in push events and pull requests. You can make sure that every code you accept meets requirements.

# What is in this repository
This repository contains a single [YAML file](sources/nginx-deployment.yaml) (in sources directory) which is used as an example Kubernetes manifest. Kubescape supports multiple YAMLs and multiple Helm charts. It finds them according to the command line definition (the positional arguments).

This repository also contains an "exception definition file" called [exceptions.json](.github/assets/kubescape/exceptions.json). It contains all the exceptions the developer wanted to apply to the validation process. See more about exceptions [here](https://github.com/armosec/kubescape/blob/master/examples/exceptions/README.md)

At last, the repository defines a YAML scanning process which can be seen [here](.github/workflows/build.yaml), you can see the way Kubescape is invoked.

# Workflow
Kubescape finds potential security problems in you YAML files. You should go over the results and you can:
1. Fix the issue - you can run Kubescape with `--verbose` flag and get assistance of how to solve a given problem
2. Create an exception for the issue - this should happen if the problem is not solvable or not interesting (see the [exceptions.json](.github/assets/kubescape/exceptions.json))









