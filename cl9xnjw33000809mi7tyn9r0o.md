---
title: "Master Creating and Deploying Lambda Functions with AWS CDK"
seoDescription: "Create, deploy, and test Lambda functions with AWS CDK using TypeScript. Learn manual testing, error troubleshooting, and AWS Console"
datePublished: Tue Nov 01 2022 03:30:30 GMT+0000 (Coordinated Universal Time)
cuid: cl9xnjw33000809mi7tyn9r0o
slug: master-creating-and-deploying-lambda-functions-with-aws-cdk
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1667273387998/PMgOwMDbH.png
tags: aws-lambda, aws-cdk

---

We'll be writing our first `lambda` function. In order to do that, let's run:

* `npm install --save @aws-cdk/aws-lambda`
    

Since we are using `typescript` we'll also:

* `npm install --save @types/aws-lambda`
    

👍 We needed to install `aws-lambda` separately because it's not part of the core AWS package. See which other constructs/packages are available in CDK [here](https://docs.aws.amazon.com/cdk/api/latest/docs/aws-lambda-readme.html).

Inside our `todo-app` will create a new directory called `lambda` and inside of it, a `hello.ts` file with the following contents.

```ts
// we are going to call this function via APIGatewayEvent which is used for HTTP requests
exports.handler = async function(event: AWSLambda.APIGatewayEvent) {
    // this is a neat trick to prettify the console log
    console.log("request:", JSON.stringify(event, null, 2));

    // this is what calling this lambda function will return
    return {
        statusCode: 200,
        headers: { "Content-Type": "text/plain" },
        body: `Hello, nothanii friends!`
    };
};
```

We need to import the lambda function into our stack file:

* `import * as lambda from "@aws-cdk/aws-lambda";`
    

Then we'll create an instance of our lambda construct, with three arguments:

1. scope (in which the construct is created): usually `this`
    
2. `id`
    
3. `props` objects (`code`, `handler` and `runtime` are obligatory)
    

```ts
const helloLambda = new lambda.Function(this, "HelloLambda", {
  // where our code is located (inside the lambda directory)
    code: lambda.Code.fromAsset("lambda"),
    // the function executed whenever this lambda function is triggered (the handler function inside hello.ts file)
    handler: "hello.handler",
    // most recent node
    runtime: lambda.Runtime.NODEJS_12_X,
});
```

Run `cdk diff` to see the two new resources:

```bash
Resources
[+] AWS::IAM::Role HelloLambda/ServiceRole
[+] AWS::Lambda::Function HelloLambda
```

Then `cdk deploy`

👍 Note, if you get the following error (I did): `Error: This stack uses assets, so the toolkit stack must be deployed to the environment`, run `cdk bootstrap` to finish configuring your account and avoid this error in the future.

# Review and execute a lambda function deployed with CDK in AWS Console

Let's go to the [AWS management console](https://aws.amazon.com/console/), search for Cloudformation, click on Stacks and check out what we've deployed.

Under the **Resources** we can see:

* AWS::IAM::Role
    
* AWS::Lambda::Function
    
* AWS::CDK::Metadata
    

Click on the lambda function id to explore it. You'll be able to use the entire code, which starts with `use strict` and finishes with the source map (because this code was transpiled from `typescript`).

Further down the page, you'll see **Tags** associated with this function (those were added automatically).

We currently don't have any **Triggers** so click on **Test** and **Configure test event** to test it manually. Choose [**Api Gateway AWS Proxy**](https://docs.aws.amazon.com/apigateway/latest/developerguide/set-up-lambda-proxy-integrations.html). Name the test then click **Create**.

Once you click **Test** again the lambda function will be called and you should see the `console.log` with "Hello, nothanii friends!" (or your own text).

## Resources

* [constructs/packages](https://docs.aws.amazon.com/cdk/api/latest/docs/aws-lambda-readme.html)
    
* [AWS management console](https://aws.amazon.com/console/)
    
* [Api Gateway AWS Proxy](https://docs.aws.amazon.com/apigateway/latest/developerguide/set-up-lambda-proxy-integrations.html)