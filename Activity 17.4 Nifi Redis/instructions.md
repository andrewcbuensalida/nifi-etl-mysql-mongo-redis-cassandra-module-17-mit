In a Terminal window, type the correct commands for your operating system to start Redis by following the steps in Mini-Lesson 17.5.
Provide a screenshot of your Terminal window to show that you successfully started Redis.

In a new Terminal window, start NiFi using the commands for your operating system by following the steps in Mini-Lesson 17.5. Provide a screenshot of your Terminal window to show that you successfully started NiFi.
Navigate to https://localhost:8443/nifi to open the NiFi UI. Provide a screenshot to show that you successfully opened the NiFi UI.

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