# BookSwap Storage Decisions

## 1. Data Inventory and Workload

BookSwap is a community book-sharing marketplace for residents of a Colombo apartment building.

| Data | Expected volume | Read/write pattern |
|---|---|---|
| Book listings | Up to 5,000 books per building | Reads are frequent; writes occur when books are listed or updated |
| Borrow requests | Grows as members request books | Reads and writes occur during request and approval |
| Loans | Grows over time | Read when checking history; write when loans are created or updated |
| Book photos | One optional photo per listing | Uploaded occasionally and read when viewing listings |
| Notifications and email jobs | Grows with user activity | Created during events and processed asynchronously |

## 2. Storage Choices

### Azure SQL Database
Store book listings, borrow requests, loans, and required member identifiers in Azure SQL. These records are related, and relational constraints and transactions help keep borrowing operations consistent.

### Azure Blob Storage
Store JPEG and PNG book photos in Blob Storage, not in the application database. The API must enforce the 5 MB upload limit and store only the photo reference or URL with the book record.

### Azure Cache for Redis
Use Redis as a temporary cache for frequently accessed catalogue search results. Azure SQL remains the source of truth.

### Azure Service Bus
Use Service Bus to queue notification and email jobs. Listing creation must succeed even when the email service is unavailable.

### Azure Communication Services Email
Use this service to send email notifications and the weekly digest of the ten most recently added books. Email delivery is best effort and must not block listing creation.

## 3. Source of Truth

- Book listings, borrow requests, and loans: Azure SQL Database.
- Photo files: Azure Blob Storage.
- Cached catalogue results: Azure Cache for Redis; not a source of truth.
- Pending notification and email jobs: Azure Service Bus until processed.
- Email delivery: Azure Communication Services Email.

## 4. Cache Strategy

### Cache-aside pattern

BookSwap will use Azure Cache for Redis with the cache-aside pattern.

1. When a search request arrives, the API checks Redis for the search results.
2. If the results are found (cache hit), the API returns them.
3. If the results are missing (cache miss), the API queries Azure SQL.
4. The API stores the results in Redis with a 60-second TTL and returns them.
5. When a book is added or updated, the API invalidates relevant catalogue cache entries.

### Pseudocode

```text
function searchBooks(searchParameters):
    key = createCacheKey(searchParameters)
    results = Redis.get(key)

    if results exist:
        return results

    results = AzureSQL.searchBooks(searchParameters)
    Redis.set(key, results, TTL = 60 seconds)
    return results
```

### What to cache and why

Cache frequently used catalogue search results, including search and condition-filter combinations. A 60-second TTL balances faster searches with reasonably fresh catalogue information. Invalidate affected catalogue entries when a book is created, updated, or becomes unavailable.

Do not cache authorization decisions or sensitive personal details. Redis is a performance layer, not the source of truth.

If Redis is unavailable, the API should fall back to Azure SQL, subject to normal database capacity limits.

## 5. Asynchronous Queue and Failure Handling

### Queue design

Azure Service Bus will hold background jobs for email notifications and the weekly email digest. The API should not wait for email delivery before confirming that a book listing was created.

A reliable implementation can use the transactional outbox pattern: save the book and an outbox event in the same Azure SQL transaction, then publish the event to Service Bus using a background publisher.

### If the consumer is down for 30 minutes

Messages remain in the Service Bus queue while the consumer is unavailable, subject to the configured message lifetime and queue capacity. Book listing creation continues because it does not depend on email delivery. When the consumer returns, it processes pending messages. Queue age and message count should be monitored so delays can be detected.

### Retry and dead-letter policy

For transient failures, retry message processing up to five delivery attempts, using increasing delays between attempts. If processing still fails, move the message to the dead-letter queue for investigation and controlled reprocessing. Use message IDs and idempotent processing to reduce duplicate notifications when messages are delivered more than once.

### Weekly digest

A scheduled job should identify the ten most recently added books each week and queue digest email jobs. Digest delivery is best effort and should not affect book listing or borrowing operations.

## 6. HTTP 5xx Retry Policy

Clients may retry a request after a temporary 5xx server error, using exponential backoff and a small random delay to avoid overwhelming the service. They should limit retries and stop after a configured maximum, while respecting the `Retry-After` header when provided. For operations that create resources or trigger side effects, use an idempotency key or another deduplication mechanism before retrying to avoid duplicate records or actions.

## 7. Database Alternatives and Workload Assumptions

### Workload assumptions

The target catalogue contains up to 5,000 books per building. Catalogue searches are expected to be more frequent than listing creation or loan updates because residents browse books repeatedly.

For initial capacity planning, assume approximately 10 catalogue reads for every write operation. This is a planning assumption, not measured production traffic, and should be revised using actual monitoring data.

### Why Azure SQL instead of Cosmos DB?

Azure SQL is the preferred primary store because books, borrow requests and loans have clear relationships and require consistent updates. Relational constraints and transactions help prevent conflicting borrowing operations.

Cosmos DB is a suitable alternative for flexible document-oriented data and certain globally distributed workloads. However, BookSwap's relational data and transaction requirements make Azure SQL the simpler starting choice for this building-scale application.

## 8. Photo Storage and Privacy

Book photos must be JPEG or PNG files with a maximum size of 5 MB. Store image files in Azure Blob Storage rather than in the application database. Azure SQL should store only the photo reference or URL.

Addresses and phone numbers must never be returned in API responses. API response schemas should expose only the information required for each operation, and access to borrower history must be restricted to authorized users.

## 9. Reliable Notification Publishing

Use the transactional outbox pattern to keep database changes and notification events consistent. Save the book listing and its outbox event in the same Azure SQL transaction. A background publisher sends pending events to Azure Service Bus and marks them as published after successful delivery; failed publishing attempts remain eligible for retry.

Consumers should process messages idempotently because a message can be delivered more than once.