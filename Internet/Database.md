- Primary key
	- mandatory unique identifier for each record
	- only one per table
	- enforces data integrity and ensures no duplications
	- creates an index automatically

- Index
	- optional performance booster
	- can have multi per table
	- make queries faster
	- don't enforce uniqueness

- Transactions
	- independent of SQL. It's just that SQL databases like PostgreSQL have had decades to perfect their transaction implementations, while NoSQL databases added them more recently and often with more limitations.

- PostgreSQL's strong transaction support comes from two main aspects:
	1. **SQL Support**
	2. ACID Properties: fully ACID compliant at all times
		1. **A**tomicity: "all-or-nothing", either the whole sql gets executed successfully or not succeed at all
		2. **C**onsistency: Database remains valid after transaction
		3. **I**solation: Concurrent transactions don't interfere
		4. **D**urability: Committed changes are permanent

- MongoDB transactions
	- Limitations:
		- Transactions across multiple shards can impact performance significantly
		- Higher latency compared to single-document operations
		- Time limits on how long transactions can run
		- More complex to implement and manage
		- Not all operations are allowed in transactions
		- Less mature implementation compared to PostgreSQL

- DynamoDB transations
	- Can only include up to 25 items per transaction
	- Higher latency and cost (consumes more capacity units)
	- Limited to specific operations (Put, Update, Delete, Get)
	- Only supports items within same region
	- No isolation levels like PostgreSQL
	- Cannot span across multiple tables in all scenarios

But no-sql db's strengths outweigh the need for strong transactions:
- High-Volume Data / Scalability
	- SQL needs to maintain relationships, while NoSQL doesn't need this
	- **Read Operations**: NoSQL usually can get all needed data from one server, SQL might need data from multiple servers
	- **Write Operations**: NoSQL's each insert is independent, SQL needs to maintain indexes and foreign keys
- Flexible Schema
	- NoSQL just adds new fields as needed, SQL needs to alter table
- Horizontal Scaling
	- SQL gets complicated if tables distributed across server

So, use PostgresQL when need:
- complex transactions
- complex joins
use MongoDB when need:
- Flexible document structure
- Fast reads and writes for document-based data
use DynamoDB when need:
- Auto-scaling for massive workloads: Can handle millions of these operations per second
- Predictable performance at any scale

This is why companies with massive scale (like Facebook, Amazon) often use:
- NoSQL for high-volume operations (user actions, events)
- PostgreSQL for critical transactions (payments, user accounts)
- Each database type where it performs best!
Example:
```javascript
// Social Media Platform Architecture

// PostgreSQL for user accounts (needs transactions)
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  email TEXT UNIQUE,
  password_hash TEXT
);

// MongoDB for posts (needs flexible schema)
{
  post_id: "123",
  content: "Hello!",
  media: [
    { type: "image", url: "..." },
    { type: "video", url: "...", duration: 30 }
  ],
  reactions: {
    like: 42,
    heart: 10,
    custom_emoji_123: 5
  }
}

// DynamoDB for user sessions (needs high throughput)
{
  session_id: "xyz",
  user_id: "123",
  last_active: timestamp,
  device_info: {...}
}
```