#AWSCognitoSampleDeveloperAuthenticationSample

This is a sample application for running a service that authenticates users, to demonstrate the developer authenticated feature of Amazon Cognito. Currently, this sample code can be used in conjunctions with the Mobile SDKs for iOS and Android. Follow the quick start steps to deploy the sample quickly using Amazon CloudFormation. To build the sample yourself and deploy using Amazon Elastic Beanstalk follow the "Building and deploying the sample yourself" section.

#Creating an identity pool
* If you have not created an identity pool that supports developer authenticated identities, follow these instructions, else skip to next step.
* Go to [Amazon Cognito Console](https://console.aws.amazon.com/cognito/home), click on 'New  Identity Pool'.
	* Provide a valid identity pool name.
	* In the Developer Authenticated Identities section, provide a developer provider name which you want to use for your application. (E.g. login.myapp).
	* Click 'Create Pool'.
* To allow Cognito to create roles on your behalf for the identity pool, click 'Update Roles'.
* Use the identity pool id provided in your sample by following the ReadMe instructions of the sample (Android/iOS) you want to try.

#Editing an identity pool
* If you have an identity pool that does not support developer authenticated identities, follow these instructions
* Go to [Amazon Cognito Console](https://console.aws.amazon.com/cognito/home), open the identity pool you want to edit and click on 'Edit Identity Pool'
	* In the Developer Authenticated Identities section, provide a developer provider name which you want to use for your application. (E.g. login.myapp).
	* Click 'Save Changes'.
* Use the identity pool id provided in your sample by following the ReadMe instructions of the sample (Android/iOS) you want to try.

#Quick Start to run the server application
* Go to [Amazon CloudFormation console](https://console.aws.amazon.com/cloudformation/home) and click on 'Create Stack'.
* Provide the stack name and this [S3 URL](https://s3.amazonaws.com/cognito-developer-authentication-sample/AWSCognitoDeveloperAuthenticationSampleCFN.json) as a template. Click 'Next'.
* Provide the identity pool id and developer provider name, click 'Next'.
* Click 'Next' on the options page.
* Acknowledge the template to have capabilities to create AWS IAM resources. Review your configuration and click 'Create'.
* Please allow the stack to finish creation of all the resources. Once the stack is created, it will give the ApplicationURL in the output tab. Use this URL in the Android/iOS sample.
* You can register users using any web browser using ElasticBeanStalkApplicationURL/jsp/register.jsp

#Building and deploying the server application yourself
* Build the sample to get the war file using "maven clean install". You will need [Apache Maven](http://maven.apache.org/download.cgi) installed on your system to run this command.
* Go to AWS Console for [ElasticBeanStalk](https://console.aws.amazon.com/elasticbeanstalk).
* Click on 'Create New Application'
	* Choose an application name and a description
* Configure the Environment 
	* Choose Environment tier as 'Web Server'
	* Choose Predefined Configuration as 'Tomcat'
	* Choose any Environment type. Click Next.
* For the source choose 'Upload' option and upload the generated CognitoDevAuthSample.war
* Choose your deployment batch size. Click Next.
* Provide an environment name, url and description. Click next.
* No additional resources are required, click next.
* On the configuration page leave the default configuration and click next.
* This sample doesn't require any environment tags, click next.
* Review and launch.
* Go to Configuration tab in the Elastic Beanstalk console.
* Click on Instances section. The instance profile will list an IAM role. (If not, then refresh and a select the role named "aws-elasticbeanstalk-ec2-role", which is created by default by ElasticBeanStalk)
	* Go to IAM console, make sure you add the role policy listed at the end of this readme to this role.
* In the [Elastic Beanstalk console](https://console.aws.amazon.com/elasticbeanstalk/home) go to Configuration, then Software Configuration.
	* Add a parameter named IDENTITY_POOL_ID, with the value of identity pool id which you have configured using the Amazon Cognito Console.
	* Add a parameter named DEVELOPER_PROVIDER_NAME, with the developer provider name which you used while creating this identity pool in the Amazon Cognito Console
    * Optionally add a parameter named REGION with the region to use. Cognito is available in 'us-east-1' and 'eu-west-1'.
* You can register users using any web browser using ElasticBeanStalkApplicationURL/jsp/register.jsp

Policy for EC2 role

````json
{
"Version": "2012-10-17",
	"Statement": [
    {
      "Action": [
        "dynamodb:*",
        "cognito-identity:GetOpenIdTokenForDeveloperIdentity"
      ],
      "Effect": "Allow",
      "Resource": "*"
    },
  ]
}
````