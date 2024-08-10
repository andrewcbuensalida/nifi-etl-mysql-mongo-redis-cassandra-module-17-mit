In a Terminal window, type the correct commands for your operating system to start Redis by following the steps in Mini-Lesson 17.5.

What Does Installing from Source Mean?

The service that NiFi provides to work and perform ETL using a Redis database can only be used when you have a standalone installation of Redis on your machine. A standalone installation means that the software is able to operate independently of other hardware or software.

The easiest way to get a standalone installation of Redis is by installing it from source, that is, by installing a program without using a package manager. Essentially, this means compiling the code on your computer and installing the software directly without using hosting services such as GitHub.

How to Install Redis from Source

To install Redis from source, follow the steps below:

Install Wget, a computer program that retrieves content from web servers.

For Windows users:

Navigate to the Redis GitHub repository https://github.com/microsoftarchive/redis/releases/tag/win-3.0.504 and download the Redis-x64-3.0.504.msi file.
Install the Redis-x64-3.0.504.msi file using the default settings.
Open a Windows CLI and run the redis-cli.

Provide a screenshot of your Terminal window to show that you successfully started Redis.

In a new Terminal window, start NiFi using the commands for your operating system by following the steps in Mini-Lesson 17.5. 

How to Install NiFi for Redis

Because NiFi can only work with a standalone installation of Redis, you will also need to install a NiFi client on your machine. In other words, after launching Redis, you won’t be able to simply navigate to localhost:8080/nifi and start working with it.

A NiFi client can be installed locally by following the steps below:

For Windows users:
Navigate to the Java Download https://www.java.com/download/ie_manual.jsp website. Download the linked file by selecting the “Agree” option and then selecting the “Start Free Download” option. Follow the installation instructions to complete the installation.

Download this https://archive.apache.org/dist/nifi/1.15.0/nifi-1.15.0-bin.zip
Getting an error with this version. It just suddenly stops after run-nifi.bat.

Try downloading 2.0.0 instead. This will error with:
Error: LinkageError occurred while loading main class org.apache.nifi.bootstrap.RunNiFi
        java.lang.UnsupportedClassVersionError: org/apache/nifi/bootstrap/RunNiFi has been compiled by a more recent version of the Java Runtime (class file version 65.0), this version of the Java Runtime only recognizes class file versions up to 61.0
My java version is java 17.0.10 2024-01-16 LTS

Now trying nifi 1.27.0. This time it stays open.

Manually extract the folder and install NiFi by following the instructions.

Starting NiFi

Once NiFi has been downloaded and installed, it can be started by using the commands appropriate for your operating system.

For Windows users:
In a command line interface, navigate to and run the following command:
..\nifi-1.15.0\bin>run-nifi.bat

Look for the nifi-app.log file in the .\nifi-1.15.0\logs folder.

Search for your credentials using the search term “Generated Username”. It will look similar to the following:

Generated Username [960e629a-c194-445e-aaba-7986dd8c7550]

Generated Password [YOIbbTLPuRuLtkwi/1BRHdSwoOYdOrMS]

Navigate to https://localhost:8443/nifi to open the NiFi UI. Use the above credentials to log in to the NiFI console from your browser. Provide a screenshot to show that you successfully opened the NiFi UI.

In the NiFi Flow process group, select the gear icon to configure the NiFi Flow process group. In the CONTROLLER SERVICES tab, select the plus sign and select RedisConnectionPoolService as the type.

Adding the RedisConnectionPoolService controller.

In the PROPERTIES tab, set the Connection String property equal to localhost:6379. Do not change any other default values. Provide a screenshot to show that you set the RedisConnectionPoolService controller settings correctly.

Inside the NiFi Flow process group, add another controller named RedisDistributedMapCacheClientService.

Adding the RedisDistributedMapCacheClientService controller.

In the PROPERTIES tab, select the RedisConnectionPoolService controller created in the previous step. Provide a screenshot to show that you set the RedisDistributedMapCacheClientService controller settings correctly.

Enable your controllers. Provide a screenshot to show that you enabled both the RedisConnectionPoolService and RedisDistributedMapCacheClientService controllers.

Create a processor titled GenerateFlowFile.

GenerateFlowFile processor.

In the PROPERTIES tab, set the Custom Text property to ${now()}. This will put the current date and time into the content of each FlowFile. In the SCHEDULING tab, change the Run Schedule to five seconds.

Provide two screenshots. The first screenshot should show that you set the values in the PROPERTIES tab correctly. The second screenshot should show that you updated the scheduling time in the SCHEDULING tab correctly.

Create a processor titled PutDistributedMapCache.

The PutDistributedMapCache processor.

In the PROPERTIES tab, set the Distributed Cache Service equal to Redis DMC Client Service. Set the Cache Entry Identifier equal to date. This will be your Redis key. Provide a screenshot to show that you set the PutDistributedMapCache controller settings correctly.

Create a processor titled FetchDistributedMapCache.

The FetchDistributedMapCache processor.

In the PROPERTIES tab, set the Distributed Cache Service equal to Redis DMC Client Service. Set the Cache Entry Identifier equal to date and the Put Cache Value in Attribute equal to date.retrieved. Provide a screenshot to show that you set the FetchDistributedMapCache controller settings correctly.

Create a processor titled LogAttribute.

The LogAttribute processor.

Provide a screenshot to show that you have all the controllers, GenerateFlowFile, PutDistributedMapCache, FetchDistributedMapCache, and LogAttribute ready in NiFi.

Connect all your processors in the order they were added by using success as the relationship type. Ensure the following:

In the PutDistributedMapCache processor settings, set Automatically Terminate Relationships equal to failure.
In the FetchDistributedMapCache processor settings, set Automatically Terminate Relationships equal to failure and not-found.
In the LogAttribute processor settings, set Automatically Terminate Relationships equal to success.
Run the flow. Provide a screenshot to show that your flow is running with the correct relationships.

In the Terminal window, navigate to the logs folder inside the libexec folder and open the nifi-app.log file by using the nano text editor.

Provide a screenshot to show that the nifi-app.log is being populated with the current date and time.