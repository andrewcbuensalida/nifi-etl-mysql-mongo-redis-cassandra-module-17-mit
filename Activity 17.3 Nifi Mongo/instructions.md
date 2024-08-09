To complete this activity, follow these steps:

Start the MongoDB and NiFi Docker containers running on the same network. Provide a screenshot of your Docker desktop to show the MongoDB and NiFi Docker containers running.

`docker network create NifiNetwork`

`docker run --name nificontainer -p 8080:8080 --network NifiNetwork -d apache/nifi:1.13.2`

`docker run -p 27017:27017 --name some-mongo --network NifiNetwork  -d mongo`

From the Docker desktop, select the CLI bash for the NiFi container. Create the output folder if it has not already been created. Provide a screenshot to show that the output directory exists in the NiFi container.

cd into /opt/nifi/nifi-current/ then
`mkdir output`

From the Docker desktop, select the CLI bash for the MongoDB container. 

From the bash prompt, enter mongosh and select Enter. You are now inside the MongoDB shell.

Populate the MongoDB database by following Steps 3 and 4 in Mini-Lesson 17.4. 

Copy and paste the command below into the mongosh shell to populate the MongoDB database:

use sample_mflix

db.movies.insertMany([
   {
      title: "Jurassic World: Fallen Kingdom",
      genres: [ "Action", "Sci-Fi" ],
      runtime: 130,
      rated: "PG-13",
      year: 2018,
      directors: [ "J. A. Bayona" ],
      cast: [ "Chris Pratt", "Bryce Dallas Howard", "Rafe Spall" ],
      type: "movie"
    },
    {
      title: "Tag",
      genres: [ "Comedy", "Action" ],
      runtime: 105,
      rated: "R",
      year: 2018,
      directors: [ "Jeff Tomsic" ],
      cast: [ "Annabelle Wallis", "Jeremy Renner", "Jon Hamm" ],
      type: "movie"
    }
])

Provide a screenshot to show that you successfully populated the MongoDB database.

From the NiFi canvas, located at http://localhost/8080/nifi, create a new process group and name it Activity17.3. Provide a screenshot to show that you successfully created the new Activity17.3 process group.

In the newly created process group, add a new processor and select the GetMongo processor type. Provide a screenshot to show that you successfully added the GetMongo processor.

Configure the PROPERTIES tab in the GetMongo processor to connect to the MongoDB database. 

In the PROPERTIES tab, set the Mongo URI field equal to mongodb://some-mongo:27017. Note that some-mongo is the name of the MongoDB container used in this example. If you used another title, then you must provide that title instead of some-mongo. Set the Mongo Database Name field equal to sample_mflix and the Mongo Collection Name field equal to movies.

Provide a screenshot to show that you successfully configured the GetMongo processor to connect to the MongoDB database.

Add a PutFile processor to the NiFi canvas. 

Right-click on the PutFile processor and select Configure. In the SETTINGS tab, select failure and success under the Automatically Terminate Relationships tab.
In the PROPERTIES tab, set the Directory field equal to /opt/nifi/nifi-current/output.

Provide a screenshot to show that you successfully added the PutFile processor.

Configure the SETTINGS and PROPERTIES tabs in the PutFile processor to create files in the output directory as shown in Mini-Lesson 17.4. 

Right-click on the PutFile processor and select Configure. In the SETTINGS tab, select failure and success under the Automatically Terminate Relationships tab.

In the PROPERTIES tab, set the Directory field equal to /opt/nifi/nifi-current/output.

Provide two screenshots. The first screenshot should show that you set the values in the SETTINGS tab correctly. The second screenshot should show that you updated the scheduling time in the PROPERTIES tab correctly.

Create a connector between the GetMongo and PutFile processors. Configure the connector as shown in Mini-Lesson 17.4. 

Connect the GetMongo and PutFile processors. Select failure, original, and success for the relationships.

Provide a screenshot to show that you successfully created and configured the connector between the GetMongo and PutFile processors.

In the NiFi UI, run the GetMongo and PutFile processors. Provide a screenshot of the NiFi canvas to show that the GetMongo and PutFile processors are running.

From the Docker desktop, select the CLI to open a bash window for the NiFi server. From the bash window, navigate to the /opt/nifi/nifi-current/output folder and list the files in the folder. Verify that the process is creating files. Provide a screenshot to show that the /opt/nifi/nifi-current/output directory has files that are being populated.

Use the cat command to display the contents of one of the files. Provide a screenshot to show that you successfully issued the cat command to display the contents of one file.