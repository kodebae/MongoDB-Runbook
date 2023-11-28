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

## 6. Indexing

