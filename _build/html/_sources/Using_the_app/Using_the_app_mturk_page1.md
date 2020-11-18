# Using the app with Amazon MTurk

## SvelteTurk
I highly recommend using [SvelteTurk](https://eshinjolly.com/svelteturk/#/) (a graphical MTurk HIT-making app by Eshin Jolly) to make running `continuous-rater` as simple as possible. Download the latest version [here](https://github.com/ejolly/svelteturk/releases) and read more about it [here](https://github.com/ejolly/svelteturk). However, if you are familiar with Python and want to write your own code to control the process, read on!

## MTurk external question
In order to use an external website within MTurk, you have to use the AWS [External question](https://docs.aws.amazon.com/AWSMechTurk/latest/AWSMturkAPI/ApiReference_ExternalQuestionArticle.html) protocol. This entails creating a `.xml` file that contains a reference to your website (built with Netlify, or something similar). Here is the template you need to use (only edit the **YOUR URL HERE** section on the third line):

```{code-block} xml
<?xml version="1.0" encoding="UTF-8"?>
<ExternalQuestion xmlns="http://mechanicalturk.amazonaws.com/AWSMechanicalTurkDataSchemas/2006-07-14/ExternalQuestion.xsd">
  <ExternalURL>YOUR URL HERE</ExternalURL>
  <FrameHeight>0</FrameHeight>
</ExternalQuestion>
```
Save this file somewhere that you will reference later.

## Python and MTurk

In order to interface with MTurk using Python, you will need to install boto3 from your command line:

```
pip install boto3
```
You will also need your AWS access key id and secret access key. Learn about how to find/generate them [here](https://aws.amazon.com/blogs/security/wheres-my-secret-access-key/).

```{note}
The following section has code that will connect to your actual MTurk account and create a HIT visible to the worldwide marketplace. This means REAL MONEY IS INVOLVED. If you would simply like to test your HIT, see the Sandbox section below. 
```

I recommend using a Jupyter Notebook to execute the following code. This part will allow Python to reference your MTurk account:


```{code-block} python
import boto3
import xmltodict
```

```{code-block} python
mturk = boto3.client('mturk',
   aws_access_key_id = "YOUR ID HERE",
   aws_secret_access_key = "YOUR KEY HERE",
   region_name='us-east-1'                
)
```
Then, run this code to create a HIT using the `.xml` file you created earlier and filling in all necessary variables:

```{code-block} python
question = open('YOUR_XML_FILE_NAME.xml',mode='r').read()
new_hit = mturk.create_hit(
    Title = 'YOUR STUDY TITLE',
    Description = 'YOUR STUDY DESCRIPTION',
    Keywords = 'YOUR KEYWORDS',
    Reward = 'YOUR HIT PAY',
    MaxAssignments = 'NUMBER OF SUBJECTS',
    LifetimeInSeconds = 'TIME BEFORE HIT EXPIRES',
    AssignmentDurationInSeconds = 'TIME TO COMPLETE HIT',
    AutoApprovalDelayInSeconds = 'TIME BEFORE HIT WORK APPROVED',
    Question = question
)

print("A new HIT has been created. You can preview it here:")
print("https://mturk.com/mturk/preview?groupId=" + new_hit['HIT']['HITGroupId'])
print("HITID = " + new_hit['HIT']['HITId'] + " (Use to Get Results)")
```



## MTurk sandbox mode

Before deploying your HIT for real, it's a good idea to run it in sandbox mode. The process of creating a sandbox HIT is very similar to the process of creating a real HIT:

```{code-block} python
import boto3
import xmltodict
```

```{code-block} python
MTURK_SANDBOX = 'https://mturk-requester-sandbox.us-east-1.amazonaws.com'
mturk_sandbox = boto3.client('mturk',
   aws_access_key_id = "YOUR ID HERE",
   aws_secret_access_key = "YOUR KEY HERE",
   region_name='us-east-1'
   endpoint_url = MTURK_SANDBOX                
)
```
Then, run this code to create a HIT using the `.xml` file you created earlier and filling in all necessary variables:

```{code-block} python
question = open('YOUR_XML_FILE_NAME.xml',mode='r').read()
new_hit = mturk_sandbox.create_hit(
    Title = 'YOUR STUDY TITLE',
    Description = 'YOUR STUDY DESCRIPTION',
    Keywords = 'YOUR KEYWORDS',
    Reward = 'YOUR HIT PAY',
    MaxAssignments = 'NUMBER OF SUBJECTS',
    LifetimeInSeconds = 'TIME BEFORE HIT EXPIRES',
    AssignmentDurationInSeconds = 'TIME TO COMPLETE HIT',
    AutoApprovalDelayInSeconds = 'TIME BEFORE HIT WORK APPROVED',
    Question = question
)

print("A new HIT has been created. You can preview it here:")
print("https://workersandbox.mturk.com/mturk/preview?groupId=" + new_hit['HIT']['HITGroupId'])
print("HITID = " + new_hit['HIT']['HITId'] + " (Use to Get Results)")
```

See the [boto3 docs](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/mturk.html) for more information on how to control your HITs via Python. 







