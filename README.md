# pg-cfn-notifications

Cloudformation is one of the most useful of all services provided by aws. With cloudformation 
we can capture our Infrastructure As Code and commit to the version control repositories.

Creating and updating infrasturcture becomes very easy with use of tools like cloudformation.
Everybody and anybody who has access to the scripts can and should be able to update the repo.
Hence infra changes becoming common like code changes. Now when our infra is updating frequently, we don't wan't to
send out bucket load of emails. To be honest emails should not be banned. Incomes another handy tool
for the developers of this age, Slack. It is one of the best tools for collaborative work culture and enables
teams to be located across continents and act as one big team.

In this pg repo, we aim to combine these two powerful tools used by modern-age developers.


## How do we do it ?

We first create a stack for our selves called notification-stack

### What is it ?

This stack called notification-stack creates:

1. SNS Topic:
    
    The Arn of this topic is used by other cfn stacsk to send stack events.

1. Lambda function:
    
    This lambda functions is triggered by the SNS topic events. It receives all the 
    stack events, transforms them into a JSON Payload and sends it off to slack
    Web hook endpoint.

## OK, How can I spin it up ?

#### Pre-requisites:

1. Create a Lambda execution role and copy the arn. (need to add this to cfn yml)
1. Create Slack incoming slack web hook and copy the URL.

#### Following are the steps to get the stack up and running.

1. Stand up the notification-stack


```commandline
aws cloudformation create-stack --stack-name notification-stack --template-body file://notification-stack/notification-stack.yml --parameters  ParameterKey=SlackWebHookUrl,ParameterValue=<ENTER YOUR SLACK WEB HOOL URL HERE> ParameterKey=LambdaCFNRole,ParameterValue=<ENTER YOUR IAM ROLE FOR LAMBDA HERE>

```

1. Once the above stack is up and running.. copy the arn of SNS topic

1. Stand up the test-notification

```commandline
aws cloudformation create-stack --stack-name test-notification-stack --template-body file://test-stack/test-stack.yml --notification-arns <SNS TOPIC ARN>
```

TODO:

1. Automate the above commands using make
1. Output the SNSTopic ARN
1. read the SNSTopic ARN when standing the test-notification-stack up.