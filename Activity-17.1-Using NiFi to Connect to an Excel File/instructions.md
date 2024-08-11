# Create a NiFi workflow to process an Excel file and convert it to a CSV file.

To complete this activity, follow these steps:

Before completing this activity, ensure that you have a Docker network, NifiNetwork, running on your machine and that a NiFi container, nificontainer, is connected to it and running.
`docker network create NifiNetwork`
`docker run --name nificontainer -p 8080:8080 --network NifiNetwork -d apache/nifi:1.13.2`

From the Docker desktop, select the CLI for the NiFi container. In the container bash window, navigate inside the opt/nifi/nifi-current folder and create two folders: input and output. Provide a screenshot to show that you successfully created the two folders: input and output.
`mkdir input`
`mkdir output`

Download the Activity17_1.xlsx Excel file to your local machine and use a Docker copy command to copy the file from your local machine to the NiFi server. Provide a screenshot to show that you successfully copied the Activity17_1.xlsx Excel file to the NiFi server.
`docker cp Activity17_1.xlsx nificontainer:/opt/nifi/nifi-current/input`

From your web browser, navigate to http://localhost:8080/nifi. Provide a screenshot to show that you are able to successfully open NiFi from the browser.

Create a new process group and name it Activity17.1. Provide a screenshot to show that you successfully created the new Activity17.1 process group.

Double-click the Activity17.1 process group. Inside the process group, drag and drop a new processor, ConvertExcelToCSVProcessor. Provide a screenshot to show that you successfully added the ConvertExcelToCSVProcessor processor to the NiFi canvas.

Modify the properties for the ConvertExcelToCSVProcessor processor to process the Activity17_1.xlsx file as it is explained in Mini-Lesson 17.2. 
In the PROPERTIES tab, set the Sheets to Extract field equal to Activity17_1.
Provide a screenshot to show that you configured the properties for the ConvertExcelToCSVProcessor processor correctly.

Add a GetFile processor. Provide a screenshot to show that you successfully added the GetFile processor to the NiFi canvas.
Configure the SCHEDULING and PROPERTIES tabs in the GetFile processor to get the Excel file for processing as it is explained in Mini-Lesson 17.2. Provide two screenshots. The first screenshot should show that you set the values in the PROPERTIES tab correctly. The second screenshot should show that you updated the scheduling time in the SCHEDULING tab correctly.

Add a PutFile processor. Provide a screenshot to show that you successfully added the PutFile processor to the NiFi canvas.
Configure the SETTINGS and PROPERTIES tabs in the PutFile processor as explained in Mini-Lesson 17.2. In the SETTINGS tab, select success to Automatically Terminate Relationships. Provide two screenshots. The first screenshot should show that you set the values in the PROPERTIES tab correctly. The second screenshot should show that you updated the scheduling time in the SETTINGS tab correctly.

Connect the GetFile processor to the ConvertExcelToCSVProcessor processor. Provide a screenshot to show that you successfully connected the GetFile and ConvertExcelToCSVProcessor processors.
Connect the ConvertExcelToCSVProcessor processor to the PutFile processor. Select failure, original, and success for the relationships. Provide a screenshot to show that you successfully connected the ConvertExcelToCSVProcessor and PutFile processors.

Start the GetFile processor. Provide two screenshots. The first screenshot should show that the GetFile processor is running (as indicated by a green arrow). The second screenshot should show that the input folder is empty (i.e., that the GetFile processor has picked up the file from the input folder for processing).
Start the ConvertExcelToCSVProcessor processor. Provide a screenshot to show that the ConvertExcelToCSVProcessor processor is running (as indicated by a green arrow).
Start the PutFile processor. Provide a screenshot to show that the PutFile processor is running (as indicated by a green arrow).
From the NiFi server bash prompt, select CLI from the Docker desktop for the NiFi server, navigate to the output folder, and list the files. Verify that the processor has created a CSV file for processing. Provide a screenshot to show that the CSV file has been created.