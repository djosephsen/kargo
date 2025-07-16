# Kargo-CSR
## The Custom StepRunner Fork

This is a fork of the excellent [Kargo](https://kargo.io) project, which tracks upstream releases while adding a single feature: A "custom" step-runner.

The general idea is that end-users (like you) write custom code in the language of your choice, wrap it in a GRPC server and run it in a pod adjacent to Kargo. Then:

1. You configure Kargo to use a "custom step" pointed at your GRPC-listening pod
```
steps = [{
          uses = "custom-grpc"
           config = {
           serverURL = "davesCustomCode.svc.cluster.local"
            }}]
```
2. When Kargo runs the step, this steprunner will serializes the freight into protobuf and ship it to your pod for processing.
3. Your pod receives the freight, makes whatever modifications it makes, and returns it along with a `stepResult` indicating whether it was successful or not.
4. When the runner receives the modified freight from your pod, it packs the returned freight into the stepContext it was given from the kargo runtime, and passes the result back upstream.

If this sounds familiar, the arrangement is analogous to [composite functions](https://docs.crossplane.io/latest/concepts/compositions/#write-a-composition-function) in Crossplane, which enable end-users to implement whatever transformation they needed, in whatever programming language they're familiar with (that support GRPC). 

## Getting Started
There is no getting started yet sorry, the custom steprunner itself is still under development, 

This repository tracks the upstream Kargo project, adding only the custom steprunner. Once the steprunner is operational, I'll publish it in a `Kargo-CSR` helm chart with identical versioning to the upstream releases.

Current work is ongoing in the `dj/grpc` branch if you wanna check it out.

Once I have the steprunner working and published, I'll to cut a second repository for client-tools, like an SDK or at least some bootstrapping scripts to make it easier to create custom functions of your own. I'll likely target Golang initially.

## Contributing
yes please
