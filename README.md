# LocalServer
local_server: A guide to setting up and running a local server on your PC for development purposes

🛠️ Local Server Setup Guide

This guide walks you through the process of setting up a local server on your machine, including configuring DynamoDB Local, deploying your backend with Loclx, and using Redis for queuing.

Prerequisites
DynamoDB Local: A local version of Amazon DynamoDB.

Loclx: A tool to expose your local server to the internet.

Redis: A powerful in-memory data store to be used as a queue.

Steps to Set Up
1. Set Up DynamoDB Local on Your PC
First, you need to install DynamoDB Local for your local database.

Download the DynamoDB Local JAR file from Amazon's official website.

Extract the files to a folder on your machine.

Now, start DynamoDB Local by running the following command:


java -Djava.library.path=./DynamoDBLocal_lib -jar DynamoDBLocal.jar -dbPath ~/Desktop/server/db
Note: Replace ~/Desktop/server/db with the correct path to your local database directory.

2. Deploy Backend Using Loclx
Now, we need to deploy the backend using the Loclx service. This tool helps to expose your locally running server to the internet.

First, install Loclx if you haven't already.

Run the following command to set up the tunnel configuration:


./loclx tunnel config -f ./rafting-video-editing-service/serverconfig.yaml
3. Create Your serverconfig.yaml
Create a serverconfig.yaml file with the following contents:

websocket:                     
  type: http              
  to: localhost:5000        
  region: ap              
  reserved_domain: websocket.editing.droame.com 
  plugins:                
    https_redirect: true  
    rate_limit: 20        # Limit incoming requests to 20 per second
    # key_auth: eijnfcmhhdtbcnoscizsgmlyujqjklpa
 
editing:                     
  type: http              
  to: localhost:3000        
  region: ap              
  reserved_domain: editing.droame.com 
  plugins:                
    https_redirect: true  
    rate_limit: 30        # Limit incoming requests to 30 per second
Note: Replace the localhost:5000 and localhost:3000 with the ports your backend services are running on.

4. Install and Set Up Redis
Redis will be used for handling queues in your local server environment.

Install Redis: Install Redis on your machine if it's not installed already. You can follow the instructions on the Redis official website for your specific operating system.

Start Redis Server:

redis-server --daemonize yes --protected-mode no

Specefic Port :-
redis-server --port 6378 --daemonize yes --protected-mode no
Connect to Redis:

redis-cli -h 127.0.0.1 -p 6378
Check if Redis is Running and Its Port:

ps aux | grep redis-server
To check Redis server status:


netstat -tulnp | grep redis
If Redis is not running, you can start a new instance:


redis-server --port 6378 --daemonize yes --protected-mode no
5. Final Steps
Once DynamoDB, Loclx, and Redis are set up and running, you can now proceed to use the backend services on your local machine. The services will be exposed to the internet using Loclx, and you will be able to queue tasks using Redis.
