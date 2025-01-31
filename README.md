# Deployment and orchestration of the Dockercoins application
 Mining application Kubernetes orchestration – DockerCoins App

# Context
Generate bitcoins
Mining is the process by which Bitcoin transactions are secured. To this end
miners perform mathematical calculations for the Bitcoin network with their computer hardware. As a reward for their services, they collect newly created bitcoins
as well as the fees for the transactions they confirm.
Miners (or mining cooperatives) compete and their income is
proportional to the computing power deployed.
— bitcoin mining, https://bitcoin.fr/minage/
Dockercoins
Our example application is Dockercoins, a Dockercoins mining application!
(bitcoin simulation).
All the images of this application (fictitious and completely useless) are available on
DockerHub. It is composed of 5 components:
• rng = a web service (Python) generating random numbers as output
docker image: brahimhamdi/rng:latest
• hasher= a web service (Ruby) generating a hash of the data sent to it by HTTP POST
docker image: brahimhamdi/hasher:latest
• worker= process (Python) using rng and hasher
docker image: brahimhamdi/worker:latest
• webui= web interface (JS)
docker image: brahimhamdi/webui:latest
• redis= NoSQL database
docker image: redis:latest
# Principle
• worker asks rng to provide it with random data
• worker injects this data into hasher , hasher generates a hash, retrieved by worker,
• For each hash starting with 0, worker generates a DockerCoin
• The worker stores the generated DockerCoins in a Redis database,
• The webui displays the data added to the Redis database in real time.
Each service is called by a simple DNS name (hasher, rng, etc.). It will be our role to
start these different services with this name so that the code is valid.
# work done
 Create a new Namespace named "dockercoins", you will apply all the
work in this namspace.
 Based on the operating principle of the application described above, define
the list of objects to create to deploy the application (deployments, services, ...)
 Create and apply the yaml manifest to deploy this application.
 Check the operation of the application via its web interface (using the
web browser). What is the hash rate.
 Reserve the following resources to hash:
- CPU: 0.1
- Memory: 300 Mi
The limit of each resource is twice the reserved value.
 Configure the hasher autoscaling with the following parameters:
2
◦ scale out if the average CPU load exceeds 50%
◦ Min number of replicas: 1
◦ Max number of replicas: 5
