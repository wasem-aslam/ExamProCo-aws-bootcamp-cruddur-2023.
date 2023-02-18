# Week 0 — Billing and Architecture



During the first week of the bootcamp, I completed several tasks and learned about a variety of AWS services. These services included AWS budget ,Billing , SNS, Event Bridge among others. I also gained hands-on experience with these services through watching videos and by doing homework.

The first week of the  bootcamp provided me with a solid foundation in a variety of AWS services and allowed me to gain practical experience  through hands-on exercises.




```

✅ Destroyed and reset AWS Credentials for root and admin user 
✅ Setup MFA for Root and admin Account.
✅ Used and Tested CloudShell from AWS console.
✅ AWS CLI installed in Gitpod and added AWS Access Keys and secret Access key.
✅ Configured 2 AWS Budget using Gitpod CLI 
✅ Used the CLI from Gitpod to create a Budget and a Billing Alarm
✅ Created CI/CD Lucid Logical Architectual Diagram
✅ Created a Conceptual Design Diagram on a Real Napkin
✅ Create an Alarm via AWS CLI
✅ Setup EventBridge, hookup Health Dashboard to SNS for service health notifications
✅ Request AWS support for service limit etc
```
-----------------------||||||############################################################||||||-------------------

I have watched all the videos and really working on and learning from this bootcamp. I have spent almost 6 hours every day. 

Here is my homework.

✅ Destroyed and reset AWS Credentials for root and admin user.

I used forget password and received email and then reset the passwords for both of my acccount.


 ![](assets/Reset%20password%20.png)

 
  Assigned admin and billing permission to admin user.
  

 ![](assets/User%20permission%20attached%20.png)


✅ Setup MFA for Root and admin Account.

I also enabled Multi factor Authentication for accounts.

 ![](assets/User%20%20MFA%20enabled%20.png)




✅ Used and Tested CloudShell from AWS console.

My cloud shell showing on manmagement console.

 ![](assets/Cloud%20Shell%20.png)



✅ AWS CLI installed in Gitpod and added AWS Access Keys and secret Access key.

Here, I followed the following Guideline and able to setup Gitpord CLI. Generated acess key and security id from AWS account and just followed and used commands. After completing this just working fine even after closing gitpord and starting again. I faced issue during this but after troubleshooting find was using space before the command.  

 ![](assets/Gitpod%20CLI%20.png)




✅ Configured 2 AWS Budget

Here I followed following [guideline from AWS](https://docs.aws.amazon.com/cli/latest/reference/budgets/create-budget.html#examples)
Cretaed budget.json and notifications-with-subscribers.json in aws/json directory and also committed in my Repository.


 ![](assets/AWS%20Budget%20%20Screen%20shot.png)




✅ Created CI/CD Lucid Logical Architectual Diagram

I used first time Lucid and learned how it works and after spending sometime able to create desired following diagram.

[Click here to view direct Link of Lucid Diagram](https://lucid.app/lucidchart/e2f6c077-0638-4498-8379-7b0e02605ba9/edit?viewport_loc=131%2C-138%2C2241%2C1185%2C0_0&invitationId=inv_b1356024-5eb9-4baa-ba08-a1f422e12cb5)

![](assets/Lucid%20Logical%20Architectual%20Diagram.png)



✅ Created a Conceptual Design Diagram on a Real Napkin

I used real napkin for conceptual diagram.

![](assets/Conceptual%20Design-%20Napkin%20Diagram.png)


✅ Create an Alarm via AWS CLI

![Just followed following Guide](https://aws.amazon.com/premiumsupport/knowledge-center/cloudwatch-estimatedcharges-alarm/)

I created aws_config.json and committed in my Repo and then used folowing command.

Topic created confirmation.
![](assets/Billing%20Alarm%20Topic%20SNS.png)

Billing Alarm created using Gitpod CLI.

![](assets/Billing%20Alarm%20using%20CLI.png)


<pre><code>aws cloudwatch put-metric-alarm --cli-input-json file://aws/json/alarm_config.json
</code></pre>

Here is Alarm trigger command used on CLI.
![](assets/CLI%20command%20run%20to%20create%20watcCloud%20Alarm%20.png)

AWS Management CLI showing Alarm is triggered.

![](assets/Cloud%20Watch%20Alarm%20created-%20Evidence.png)



✅ Setup EventBridge, hookup Health Dashboard to SNS for service health notifications

Just created sns topic and Servicehealth event to trigger when there would be problem. 

Created service health topic from gitpod CLI.

![](assets/SNS%20topic%20service%20health%20using%20CLI.png)

Screenshot showing confirmation from my email done for the SNS topic subscription.

![](assets/Service%20health%20sns%20topic%20uisng%20CLI.png)

Then I simply created Event in Event Bridge using console and selected recently created topic.

![](assets/Even%20Bridge%20screenshot%201.png)

Showing the details.

![](assets/Event%20Bridge%20screnshot%202.png)


Here is Finally created EventBridge Event.

![](assets/Final%20Event%20Created.png)



✅ Request AWS support for service limit etc

I requested AWS through management console and requested.

Here is acreenshot showing received confirmation.

![](assets/Email%20received%20from%20AWS%20support.png)
