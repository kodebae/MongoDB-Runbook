# 🍃 MongoDB Runbook

## Intro
> MongoDB is a popular, open-source, NoSQL database management system that is designed to store, retrieve, and manage vast amounts of data in a flexible and scalable manner. Here's a quick introduction to some of its key features and concepts:

- NoSQL Database: MongoDB is categorized as a NoSQL database, which means it doesn't rely on a traditional relational database management system (RDBMS) structure. Instead, it uses a flexible document-oriented data model.
- Document-Oriented: MongoDB stores data in a format called BSON (Binary JSON), which is a binary-encoded serialization of JSON-like documents. These documents can contain various types of data, including key-value pairs, arrays, and even nested documents.
- Collections and Documents: Data in MongoDB is organized into collections, which are similar to tables in relational databases. Each collection contains a set of documents, and each document is a self-contained unit of data with a unique identifier called an ObjectID.
- Schema-less: MongoDB is schema-less, which means that documents in a collection can have different structures. This flexibility makes it easier to adapt to changing data requirements.
- Scalability: MongoDB is designed to be highly scalable, both vertically (by adding more resources to a single server) and horizontally (by distributing data across multiple servers or nodes). This makes it suitable for handling large and growing datasets.
- High Availability: MongoDB provides features for high availability, including replica sets. Replica sets are a group of MongoDB servers that maintain multiple copies of the data. If one server fails, another can take over, ensuring data availability.
- Rich Query Language: MongoDB offers a powerful query language for retrieving and manipulating data. You can perform various queries, including filtering, sorting, and aggregation, using its query operators.
- Indexes: MongoDB supports indexing to improve query performance. Indexes allow for faster data retrieval by creating efficient data access paths.
- Geospatial Capabilities: MongoDB includes built-in support for geospatial data, making it suitable for location-based applications and services.
- Community and Enterprise Versions: MongoDB offers both a free-to-use community edition and a paid enterprise edition with additional features and support options.
- Drivers and Ecosystem: MongoDB provides official drivers for various programming languages, making it easy to integrate into different applications and platforms. It also has a rich ecosystem of tools and services that enhance its functionality.

> MongoDB is widely used in modern web and mobile applications, as well as in big data and analytics projects, due to its flexibility, scalability, and developer-friendly features. It's a valuable tool for organizations that need to manage and process diverse and rapidly changing data.

## 🗺️ MongoDB Atlas
> MongoDB Atlas is a cloud-hosted database service provided by MongoDB for hosting and managing MongoDB databases. It allows you to deploy, scale, and manage MongoDB databases with ease. Here's a quick rundown of MongoDB Atlas along with an example in Java code:

### 1. Create a MongoDB Atlas Cluster:

To get started with MongoDB Atlas, you need to create an Atlas cluster through the MongoDB Atlas dashboard. Follow the instructions on the website to set up your cluster.

### 2. Connect to MongoDB Atlas from Java:

To connect to your MongoDB Atlas cluster from a Java application, you'll need the MongoDB Java Driver. You can add it to your project's dependencies using a tool like Maven or Gradle.

```
<!-- Maven dependency for MongoDB Java Driver -->
<dependency>
    <groupId>org.mongodb</groupId>
    <artifactId>mongo-java-driver</artifactId>
    <version>3.12.11</version> <!-- Check for the latest version -->
</dependency>
```

### 3. Java Code to Connect to MongoDB Atlas:

> Here's a basic Java code example to connect to your MongoDB Atlas cluster and perform a simple operation:

```
import com.mongodb.MongoClientSettings;
import com.mongodb.MongoCredential;
import com.mongodb.ServerAddress;
import com.mongodb.client.MongoClient;
import com.mongodb.client.MongoClients;
import com.mongodb.client.MongoCollection;
import com.mongodb.client.MongoDatabase;
import org.bson.Document;

public class MongoDBAtlasExample {
    public static void main(String[] args) {
        // Replace these values with your MongoDB Atlas cluster connection details
        String username = "your_username";
        String password = "your_password";
        String clusterUrl = "your_cluster_url";

        // Create credentials
        MongoCredential credential = MongoCredential.createCredential(
                username,
                "admin", // Default authentication database for MongoDB Atlas
                password.toCharArray()
        );

        // Set up the MongoDB client
        MongoClientSettings settings = MongoClientSettings.builder()
                .applyToClusterSettings(builder ->
                        builder.hosts(Arrays.asList(new ServerAddress(clusterUrl))))
                .credential(credential)
                .build();

        try (MongoClient mongoClient = MongoClients.create(settings)) {
            // Connect to your database
            MongoDatabase database = mongoClient.getDatabase("your_database_name");

            // Get a collection
            MongoCollection<Document> collection = database.getCollection("your_collection_name");

            // Insert a document
            Document document = new Document("name", "John")
                    .append("age", 30);
            collection.insertOne(document);

            System.out.println("Document inserted successfully!");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

> Make sure to replace "your_username", "your_password", "your_cluster_url", "your_database_name", and "your_collection_name" with your actual MongoDB Atlas credentials and database details.

> This example demonstrates how to connect to MongoDB Atlas using the Java driver, authenticate with your credentials, and perform a simple insert operation. You can build upon this foundation to perform various database operations in your Java application.


## Projections

> Query projections in MongoDB allow you to control which fields are returned in the query results. This is particularly helpful for optimizing bandwidth and improving query performance.