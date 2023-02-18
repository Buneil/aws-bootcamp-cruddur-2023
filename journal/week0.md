# Week 0 — Billing and Architecture
## Installing AWS CLI
I was able to install Aws Cli on Gitpod by running the  commands 

```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

After installing AWS CLI, you will have to configure it to use it. To configure it, you will have to provide access keys and the region.

First, let’s see how to check whether AWS CLI has been successfully installed in your system. For that, just type in the following command in your command prompt:


```
aws --version
````
![Installing Aws CLI](assets/aws%20version.png)

The above screenshot tells you that AWS CLI has been successfully installed. Now, let’s configure it. Before starting it, get your AWS access and the secret access keys. If you do not have them, follow these steps:

## Created an admin user and generated Aws Credentials with secret keys
Login to your AWS account
Click on the AWS account name ( dropdown ) on the top right.
Select My Security Credentials
Click on the Access keys option.  Click on Create New Access Key.
Copy the given key and paste it in notepad.
Run below comands

```
export AWS_ACCESS_KEY_ID="AKIA6OE6X44K7VUTDQIL"
export AWS_SECRET_ACCESS_KEY="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"
export AWS_DEFAULT_REGION="us-east-1"

```


## Conceptual Diagram in Lucid chart

I was able to recreate both logical and conceptual diagrams with Lucid chart

![Designed a conceptual crudder design](assets/crudder%20conceptual.png)

![Logical diagram](assets/Crudder%20logical%20diagram.png)



Conceptual Lucid [ chart on lucid]()
https://lucid.app/lucidchart/8fd184af-4139-438e-8bba-d948f0c613bc/edit?invitationId=inv_1ae594bf-1b99-4ee5-8beb-f5132d5c81eb&page=0_0#

Logical crudder on lucid [the logical diagram]()
https://lucid.app/lucidchart/e89bc9ab-ceff-4ef4-bfee-aeeff3b6e231/edit?viewport_loc=-554%2C66%2C2568%2C1238%2C0_0&invitationId=inv_3c49ff84-f20f-47a0-885b-58f34c23d31f




## Creating Alarms

To trigger  an Amazon CloudWatch alarm using the AWS CLI, you have to run the command aws cloudwatch set-alarm-state in your terminal
Creating an  a billing alarm in Cli you run this command.
```
aws sns create-topic --name billing-alarm
```

```
aws sns subscribe \
    --topic-arn=:"arn:aws:sns:us-east-1:992470361877:billing-alarm" \
    --protocol=email \
    --notification-endpoint=lbunei@gmail.com

```
Other option would be using a json configuration file
```
aws cloudwatch put-metric-alarm --cli-input.jason file://aws/jason/alarm-config.json

````

## How to create a budget

According to the example of cli document, the command would be like this.
```
aws budgets create-budget \
    --account-id $AWS_ACCOUNT_ID \
    --budget file://aws/json/budget.json \
    --notifications-with-subscribers file://aws/json/budget-notifications-with-subscribers.json 

````

budget.json and notifications-with-subscribers.json are used to describe the settings.


Here, I’d create an AWS Budget of 10 dollars .

Create budget.json
First, I need to set BudgetName to appear in the console. BudgetType should be COST and BudgetLimit is 50USD.
```
{
 "BudgetName": "my-budget",
 "BudgetType": "COST",
 "BudgetLimit": {
  "Amount": "10.0",
  "Unit": "USD"
 },
 "CostTypes": {
  "IncludeTax": true,
  "IncludeSubscription": true,
  "UseBlended": false,
  "IncludeRefund": false,
  "IncludeCredit": false,
  "IncludeUpfront": true,
  "IncludeRecurring": true,
  "IncludeOtherSubscription": true,
  "IncludeSupport": true,
  "IncludeDiscount": true,
  "UseAmortized": false
 },
 "TimeUnit": "MONTHLY"
}

 ```
 ![the budget created](assets/montly%20budget.png)
 
