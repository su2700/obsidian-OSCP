# MongoDB Pentesting

**`Default Port: 27017`**

**MongoDB** is a popular NoSQL database that uses a document-oriented data model. It stores data in flexible, JSON-like documents, meaning fields can vary from document to document and data structure can be changed over time.

MongoDB is widely used in modern web applications due to its flexibility, scalability, and performance.

## Connect

### Connect Using MongoDB Shell

```
mongo <target-ip>:<port>
```

### Connect Using MongoDB Compass

MongoDB Compass is a GUI tool for interacting with MongoDB databases.

## Recon

### Identifying a MongoDB Server

You can use `Nmap` to check if there's a MongoDB server on a target host like this:

```
nmap -p 27017 X.X.X.X
```

### Banner Grabbing

You can use `Netcat` to find out if a MongoDB service is running and its version by looking at the welcome message it shows when you connect. This method is called Banner Grabbing.

```
nc -nv X.X.X.X 27017
```

## Enumeration

### MongoDB Server Information

You can connect to the MongoDB server and gather information about the server, databases, collections, users, etc. using MongoDB commands.

### MongoDB Client Tools

Tools like MongoShell and MongoDump can be used for interacting with MongoDB databases and performing enumeration tasks.

## Attack Vectors

### Default Credentials

MongoDB instances often come with default credentials or no authentication enabled. It's crucial to check for default credentials or weak authentication configurations.

### NoSQL Injection

Similar to SQL Injection in relational databases, NoSQL Injection attacks target MongoDB databases by exploiting vulnerabilities in query constructions.

### Unprotected MongoDB Instances

MongoDB instances sometimes have no access control or firewall rules, leaving them exposed to unauthorized access from the internet. You can search for MongoDB instances using tools like Shodan and exploit them if they're unprotected.

## Post-Exploitation

### Common MongoDB Commands

|Command|Description|Example|
|---|---|---|
|show dbs|List all databases|`show dbs`|
|use <db>|Switch to a specific database|`use mydatabase`|
|show collections|List all collections in the database|`show collections`|
|db.collection.find()|Retrieve documents from a collection|`db.mycol.find().pretty()`|
|db.collection.insertOne()|Insert a document into a collection|`db.mycol.insertOne({name: "John", age: 30})`|
|db.collection.deleteOne()|Delete a document from a collection|`db.mycol.deleteOne({name: "John"})`|
|db.dropDatabase()|Drop the current database|`db.dropDatabase()`|

### Exfiltrating Data

Once you have access to a MongoDB database, you can exfiltrate sensitive data by querying the database and extracting the results.

### Ransomware Attacks

Attackers may encrypt MongoDB databases and demand a ransom for decryption, exploiting vulnerabilities in MongoDB instances.

### Denial-of-Service (DoS) Attacks

MongoDB instances may be susceptible to DoS attacks, disrupting database availability and causing service downtime.

**Tags:**

- [Port 27017](https://hackviser.com/tactics/tags/port-27017)