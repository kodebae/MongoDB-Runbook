# Let's dive into the realm of CRUD operations in MongoDB using Java! 🚀

## 1. Create Operation

> In this example, we connect to a MongoDB server, access a specific database and collection, create a new document, and insert it into the collection.

```
// Import necessary MongoDB Java driver classes
import com.mongodb.client.MongoClients;
import com.mongodb.client.MongoClient;
import com.mongodb.client.MongoCollection;
import com.mongodb.client.MongoDatabase;
import org.bson.Document;

public class MongoDBExample {
    public static void main(String[] args) {
        // Connect to MongoDB server
        try (MongoClient mongoClient = MongoClients.create("mongodb://localhost:27017")) {
            // Access the database named "yourDBName"
            MongoDatabase database = mongoClient.getDatabase("yourDBName");

            // Access the collection named "yourCollectionName"
            MongoCollection<Document> collection = database.getCollection("yourCollectionName");

            // Create a new document
            Document newDocument = new Document("name", "John Doe")
                    .append("age", 25)
                    .append("city", "New York");

            // Insert the document into the collection
            collection.insertOne(newDocument);

            System.out.println("Document inserted successfully!");
        } catch (Exception e) {
            System.err.println("Error: " + e.getMessage());
        }
    }
}
```
---

## 2. Read Operation

> In this example, we query the collection to find documents where the "city" field is "New York" and print the results.

```
// Assuming you have the MongoClient and collection already initialized

// Find documents where the "city" field is "New York"
Document query = new Document("city", "New York");

// Perform the find operation
try (MongoCursor<Document> cursor = collection.find(query).iterator()) {
    while (cursor.hasNext()) {
        Document document = cursor.next();
        System.out.println(document.toJson());
    }
} catch (Exception e) {
    System.err.println("Error: " + e.getMessage());
}
```
---

## 3. Update Operation

> In this example, we're updating the document(s) where the "name" is "John Doe" to set the "age" field to 26.

```
// Assuming you have the MongoClient and collection already initialized

// Define the condition for the update
Document query = new Document("name", "John Doe");

// Define the update operation
Document update = new Document("$set", new Document("age", 26));

// Perform the update operation
UpdateResult result = collection.updateOne(query, update);

// Print the result
System.out.println("Matched " + result.getMatchedCount() + " document(s) and modified " + result.getModifiedCount() + " document(s).");
```
---

## 4. Delete Operation

> In this example, we're deleting the document where the "name" is "John Doe."

```
// Assuming you have the MongoClient and collection already initialized

// Define the condition for the delete
Document query = new Document("name", "John Doe");

// Perform the delete operation
DeleteResult result = collection.deleteOne(query);

// Print the result
System.out.println("Deleted " + result.getDeletedCount() + " document(s).");

```
---

## 5. Aggregation Operations

> Aggregations allow us to perform data transformations and computations on the data stored in MongoDB. In this example, we're using the aggregation pipeline to match documents where the "city" is "New York" and then group them by city, calculating the average age for each city.

```
// Assuming you have the MongoClient and collection already initialized

// Define the aggregation pipeline
List<Bson> pipeline = Arrays.asList(
        // Match documents where the "city" is "New York"
        Aggregates.match(Filters.eq("city", "New York")),
        // Group documents by "city" and calculate the average age for each city
        Aggregates.group("$city", Accumulators.avg("averageAge", "$age"))
);

// Execute the aggregation pipeline
try (MongoCursor<Document> cursor = collection.aggregate(pipeline).iterator()) {
    while (cursor.hasNext()) {
        Document result = cursor.next();
        System.out.println(result.toJson());
    }
} catch (Exception e) {
    System.err.println("Error: " + e.getMessage());
}
```

> In this example, we're aggregating (playing) with the blocks by matching specific criteria, grouping them by shape, and counting how many blocks are in each group.

> Imagine you have a big box of colorful building blocks. Each block represents a piece of information in your MongoDB.

Now, let's play with these blocks using Aggregation:

### Filter (Match):
You decide to only play with the red blocks. That's the first step - filtering. In MongoDB, this is like picking specific pieces of information based on certain conditions.

### Group (Group):
Next, you decide to group the red blocks by shape. You put all the square red blocks together and all the circular red blocks together. In MongoDB, this is grouping information by a specific field, like "city" in our previous example.

### Calculate (Aggregate):
Now, for each group, you count how many blocks there are. For the square red blocks, you count 5, and for the circular red blocks, you count 3. In MongoDB, this is similar to performing calculations on grouped data, like finding the average age for each city.

### Result (Output):
Finally, you have a summary - you know how many square red blocks and circular red blocks you have. In MongoDB, this is the final result after aggregating the data based on your chosen operations.

```
// Assuming you have the MongoClient and collection already initialized

// Define the aggregation pipeline
List<Bson> pipeline = Arrays.asList(
        // Match documents where the "color" is "red"
        Aggregates.match(Filters.eq("color", "red")),
        // Group documents by "shape" and count each group
        Aggregates.group("$shape", Accumulators.sum("count", 1))
);

// Execute the aggregation pipeline
try (MongoCursor<Document> cursor = collection.aggregate(pipeline).iterator()) {
    while (cursor.hasNext()) {
        Document result = cursor.next();
        System.out.println(result.toJson());
    }
} catch (Exception e) {
    System.err.println("Error: " + e.getMessage());
}

```
---

## 6. Indexing

> Think of your MongoDB collection as a giant bookshelf with lots of books.

<em> Without Indexing (The Search Adventure):

* Imagine you want a specific book but don't know where it is. Without indexes, it's like searching through every book on the shelf until you find the right one.
With Indexing (The Magic Bookmarks):

* Indexing is like having a magical table of contents at the beginning of the bookshelf. It tells you exactly which books are on each shelf.
When you want a book about animals, you check the table of contents, go straight to the 'Animals' section, and find your book instantly!
In MongoDB Terms:

* Your books are the data in your MongoDB collection.
Indexes are like those magical bookmarks that help MongoDB quickly locate the data you're looking for. </em>

> In this example below, we're creating an index on the "bookTitle" field, making it easier and faster to find books based on their titles.

```
// Assuming you have the MongoClient and collection already initialized

// Create an ascending index on the "bookTitle" field
collection.createIndex(Indexes.ascending("bookTitle"));

```
---

## 7. Sharding

<em> Imagine your MongoDB collection is like a massive library with books on various subjects.

Without Sharding (One Enormous Shelf):

* If your library is so huge that it won't fit on one shelf, it becomes challenging to find the right book quickly. It's like searching through an enormous shelf with no specific order.
With Sharding (Organizing by Categories):

* Sharding is like having multiple smaller bookshelves, each dedicated to a specific category or author. For example, one shelf is for Science Fiction, another for History, and so on.
When you want a Science Fiction book, you know exactly which shelf to check, making it faster and more organized.
In MongoDB Terms:

* Your books represent the data in MongoDB.
Sharding is like splitting your MongoDB collection into smaller chunks based on a chosen key (like category or author). </em>

### Setting up sharding in Mongo DB

> In this example, we're enabling sharding for a specific database and then sharding a collection based on the "category" field.

```
// Assuming you have the MongoClient already initialized

// Enable sharding for a specific database
MongoDatabase adminDb = mongoClient.getDatabase("admin");
Document enableShardingCommand = new Document("enableSharding", "yourDatabaseName");
adminDb.runCommand(enableShardingCommand);

// Shard a collection based on a chosen key, for example, "category"
MongoDatabase yourDb = mongoClient.getDatabase("yourDatabaseName");
MongoCollection<Document> yourCollection = yourDb.getCollection("yourCollectionName");
Document shardKey = new Document("category", 1);
Document shardCommand = new Document("shardCollection", "yourDatabaseName.yourCollectionName")
        .append("key", shardKey);
adminDb.runCommand(shardCommand);

```
---

## 8. Replication

<em> Imagine your MongoDB data as a magical storybook.

Without Replication (The Lone Storybook):

* You have one incredible storybook, but what if it gets lost or damaged? There's only one copy, and if something happens to it, the story might be lost forever.
With Replication (Copies for Safety):

* Replication is like having magical duplicates of your storybook. You have the same enchanting story in multiple copies.
If one copy gets lost or damaged, no worries! You have another one ready to continue the adventure.
In MongoDB Terms:

* Your storybook represents the data in MongoDB.
Replication is like creating multiple copies (replica sets) of your MongoDB data to ensure its safety and availability. </em>

### Setting up replication in Mongo DB

```
// Assuming you have the MongoClient already initialized

// Create a replica set with three members
List<ServerAddress> members = Arrays.asList(
        new ServerAddress("server1", 27017),
        new ServerAddress("server2", 27017),
        new ServerAddress("server3", 27017)
);

MongoClientSettings settings = MongoClientSettings.builder()
        .applyToClusterSettings(builder ->
                builder.hosts(members))
        .build();

MongoClient mongoClient = MongoClients.create(settings);

```
---

## Projections

> Query projections in MongoDB allow you to control which fields are returned in the query results. This is particularly helpful for optimizing bandwidth and improving query performance.
