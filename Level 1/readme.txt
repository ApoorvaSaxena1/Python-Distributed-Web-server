The year is 50 B.C. Gauls, led by Asterix, look forward to challenging the Romans in the latest edition of the ancient Olympic games. Gauls have made tremendous technological progress since the previous olympics and have finally entered the "smart" stone-age. Every Gaul now has a stone tablet that gives them the latest sports scores of their fellow villagers. Each smart-stone (not to be confused with a smart-phone, which are yet to be invented) is updated by Obelix (a sculpter by profession), who has tasked by the village chief to disseminate the latest scores by engraving them onto each stone tablet. You are tasked to design a distributed system using a client-server architecture to efficiently disseminate latest scores to the villagers. Assume that there are N tablets, each of which is a client, that needs to be periodically updated with sports scores. (N should be configurable in your system).

First design a client-pull architecture where each client tablet refreshes the medal tally by periodically pulling the latest data from Obelix's stone server. Additionally a client tablet can also request the current score for an ongoing sports event and receive a response from the server.

Cacofonix, the village bard, is responsible for providing Obelix's server live updates from the olympic stadium, which he does by singing the scores and thereby sending updated scores to the server. Since Gauls are restful by nature, they have chosen to use the RESTful API for client-sever communications. The RESTful interface provides a simple method for RPC-like communication over HTTP.

Your system should implement the following REST endpoints:

SERVER_IP:80/getMedalTally/teamName
This should return a json object that contains the number of gold, silver and bronze leafs earned by the specified teamName. Your system should track medal tallies for at least two teams, namely Gual and Rome.

SERVER_IP:80/incrementMedalTally/teamName/medalType/auth_id
This increments the medal tally for the specified team, and medalType can be bronze, silver, or gold and the appropriate medal count for that team is incremented. The auth_id variable should be an id unique to Cacofonix used to authorize the action. This should return a success message on successful increment by Cacofonix and a failure message if anyone other than Cacofonix tries to increment the tally.

SERVER_IP:80/getScore/eventType
This should return a json object with the latest score for each team for the specified eventType. Pick at least three winter Olympics events such as stone Curling and stone skating and provide scores for these events.

SERVER_IP:80/setScore/eventType/rome_score/gaul_score/auth_id
This updates the current score for the specified event. The auth_id variable should be an id unique to Cacofonix used to authorize the action. The variables gaul_score and rome_score should be the new score values for each team. This should return a success message on successful update by Cacofonix and a failure message if anyone other than Cacofonix tries to update the score.

While any stone tablet client can request scores or medal tallies (and can do so concurrently), only the Cacofonix's update process can increment medal tallies and scores in the system. This can be accomplished by sending an authorization id with update requests that you can use to verify permissions. Design each tablet as a separate process, the server is its own process and so is Cacofonix update process. Your client-pull system should be able to handle multiple concurrent requests from different tablets and also concurrent queries and updates. Thus you should implement python threading in the sports server and also use proper synchronization at the server to ensure correctness. You are free to use any server architecture to handle concurrent requests, such as a thread per request model, a pre-spawned pool of threads or event-based architectures.

Since stone curling is a very popular event among the Gauls, the chief has decreed that a server-push architecture be used to efficiently and immediately disseminate score updates to each tablet. Before a curling event begins, any Gaul interested in receiving push notifications registers with the server. When a curling event is in progress, updates are pushed from the server to all registered tablets whenever any update is received from Cacofonix's update process.

Your system should implement the following interfaces for server-push mode:

SERVER_IP:80/registerClient/clientID/eventType
This registers a client tablet to receive push updates for the specified eventType.

Client_IP:80/pushUpdate/variable1/variable2/variable3/...etc
This should be called for all registerd clients and pushes an update of the current state to all registered clients.

For the push mode, you can implement a separate application to run on the client tablet than the above pull-based application. That is, there is no need to write an integrated client that provides both push and pull functions, although you are certainly allowed (and encouraged) to do so.

REST Interface
As noted above, your system should use a REST client/server architecture. This means creating several endpoints that correspond to the interfaces provided above.

A sample endpoint for the getMedalTally(teamName) interface may be of the following form:
serverIP:80/getMedalTally/teamName
Clients can make this api call to the server and will receive back a json object of the following form:
 {
 	"medals": {
 		"bronze": 0, 
 		"gold": 2, 
 		"silver": 5
 	}
 }  
This can be extended to update scores and register clients for push mode.
While we recommend using GET requests for simplicity, feel free to use PUT/POST requests if you find them easier to use.
While python web frameworks such as Django or Flask make is simple to write web application using REST interfaces, in this case, you should write your own code rather than using a python web framework. However, you are encouraged to use a simple library such as RESTArt to implement the REST interface. In addition, you should use Python Threads interface (see Python 3 Threading or Python 2 Threading) to implement a multi-threaded sports server to handle concurrent requests. Of course, an event-based server with non-blocking I/O can also be used for concurrency if you so wish.
Other requirements:

For your multi-threaded server, be aware of the thread synchronizing issues to avoid inconsistency, race conditions or deadlock in your system.
No GUIs are required. Simple command line interfaces and textual output of scores and medal tallies are fine.
A secondary goal of this and subsequent labs is to make you familiar with modern software development practices. Familiarity with source code control systems (e.g., git, mercurial, svn) and software testing suites is now considered to be essential knowledge for a Computer Scientist. In this lab, you will use github for a source code control repository. You will need to create a free student github account and work on this project using a repository that we have created using github classroom. Please make sure you use multiple commits and provide detailed commit messages. You will be evaluated on your use of effective commits. Instructions for doing so as well as as pointers to a git tutorial are here.
A related goal is to ensure you properly test you code. You can create specific scenarios and/or inputs to test your code and verify that it works as expected. Unit testing frameworks are fine to use here, but do keep in mind that this is a distributed application with peers running on different machines. So testing is more complex in this setting. For the purposes of the lab, you should write three tests of your choice either using a testing framework or using your own scripts/inputs to test the code. The tests and test output should be submitted along with your code.
We do not expect elaborate use of github or testing frameworks - rather we want you to become familiar with these tools and start using them for your lab work (and other programs you write).
