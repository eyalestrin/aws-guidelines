# How to create budget and billing alerts

1. Login to the Billing console:

   https://console.aws.amazon.com/billing/

2. From the left pane, click on Budgets -> Create budget

3. Click on Create a budget

   + Budget type – Cost budget

   + Click “set your budget”

   + Set your budget page:

     + **Name**: Budget notification policy for account **<Account_ID>**

       Note: Replace AWS Account ID with your actual account ID, as appear inside

       https://console.aws.amazon.com/billing/home#/account

     + **Period:** Annually

     + **Budget effective dates:** Expiring Budget

     + **Budget amount:** Fixed

     + **Budgeted amount:** Specify the entire year budget

       Note: In case there is credit for the account, add the credit to the current budget

     + **Aggregate costs by:** Unblended costs

4. Click “Configure alerts”

5. Configure alerts page:

   + **Send alert based on:** Actual Costs
   + **Alert threshold:** 75%
   + **Email contacts:** Specify here your email address or mailing list

6. Click “Confirm budget”
7. Click Create
8. Log off the AWS console