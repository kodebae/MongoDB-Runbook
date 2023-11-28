# Let's dive into the realm of CRUD operations in MongoDB using Java! 🚀

## 1. Create Operation

In this example, we connect to a MongoDB server, access a specific database and collection, create a new document, and insert it into the collection.

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

2. Read Operation

In this example, we query the collection to find documents where the "city" field is "New York" and print the results.

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

