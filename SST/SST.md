- SST:

  SST comes with a list of higher-level CDK constructs designed to make it easy to build serverless apps. They are easy to get started with, but also allow you to customize them. It also comes with a local development environment that we will be relying on through this guide. So when you run:

  sst build, it runs cdk synth internally
  pnpm sst dev or pnpm sst deploy, it runs cdk deploy

  - An SST app is made up of two parts.

  - stacks/ — App Infrastructure:

    - The code that describes the infrastructure of your serverless app is placed in the stacks/ directory of your project.
      SST uses AWS CDK, to create the infrastructure.

  - packages/ — App Code:

    - The Lambda function code that’s run when your API is invoked is placed in the packages/functions directory of your project.
      While packages/core contains our business logic.

  - SST features a Live Lambda Development environment that allows you to work on your serverless apps live.
    `pnpm sst dev`
    The first time you run this command it’ll ask you for the name of a stage.
    A stage or an environment is just a string that SST uses to namespace your deployments.

    The idea here is that we are able to work on separate environments. So when we are working in our personal environment (Jay), it doesn’t break the API for our users in prod. The environment (or stage) names in this case are just strings and have no special significance. We could’ve called them development and production instead. We are however creating completely new serverless apps when we deploy to a different environment. This is another advantage of the serverless architecture. The infrastructure as code idea means that it’s easy to replicate to new environments. And the pay per use model means that we are not charged for these new environments unless we actually use them.
