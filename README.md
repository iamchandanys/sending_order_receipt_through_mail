# Sending order receipt file to the user through mail using Azure Functions

###### Key Features:

Azure HttpTrigger, QueueTrigger, BlobTrigger, TableStorage, SendGrid

###### Requirement:

Visual Studio 2017+, .net core 2.1, Azure access

###### local.settings.json file changes:
 
 1. Add Azure Storage connection string in "AzureWebJobsStorage" section.
 2. Add Send Grid Api Key in "SendGridApiKey" section.
 3. Add From Email Id in "EmailSender" section.
 
 ```
 {
  "IsEncrypted": false,
  "Values": {
    "AzureWebJobsStorage": "UseDevelopmentStorage=true",
    "FUNCTIONS_WORKER_RUNTIME": "dotnet",
    "EmailSender": "FromEmailId",
    "SendGridApiKey": "YourSendGridApiKey"
  }
}
 ```

![header image](https://github.com/iamchandanys/sending_order_receipt_through_mail/blob/master/docs/AzureFunctionDemo.png)

###### Steps:

Step A - Receives Order Details like order id, email id, order amount etc., through HTTP trigger.

1. Need to make HTTP request once the order is placed or once the payment is done or whenever we need to generate receipt.
2. eads the request body & send a Queue Message to generate Receipt File.
3. Save the order data in Table Storage.

Step B - Receives Queue message and generates the Receipt File.

4. Queue triggers once it receives the message.
5. Generates Receipt File with the name orderid and store it in Blob Storage.

Step C - Sends Receipt File to the user.

6. Blob triggers when any new file is saved in it.
7. It reads the data from Table Storage by file name (orderid) & then sends Receipt file to the user through mail using Send Grid.
