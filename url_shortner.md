

Convert long URLs into short links and redirect users when they access the short URL.

**Example:**

```
https://amazon.com/products/very-long-id  
→ bit.ly/abc123
```

---

## Requirements

### Functional Requirements

* Shorten a long URL
* Redirect to original URL
* (Optional) Expiry support
* (Optional) Analytics tracking

### Non-Functional Requirements

* Low latency (fast redirects)
* High availability
* Scalable (millions of URLs)

---

##  High-Level Design

```
Client → API Server → Database
                ↓
           Cache (Redis)
```

---

## Key Design Decisions

### (A) Short URL Generation

#### Hashing (Not Preferred)

* Collisions possible
* Long output strings

#### Auto-increment ID + Base62 Encoding

```
ID = 1000001
Base62 → "abc123"
```

**Why Base62?**

* Uses: a-z, A-Z, 0-9
* Compact and short

---

### Database Design

**Schema:**

```
short_url | long_url | created_at | expiry
```

**Example:**

```
abc123 → amazon.com/very-long-url
```

---

###  Read Optimization (Critical)

**Redirect Flow:**

```
User hits short URL
→ Check Cache
→ Cache miss → Query DB
→ Return original URL
```

**Optimizations:**

* Redis cache for hot URLs
* CDN (optional)

---

##  Scaling the System

### Problem 1: Database Bottleneck

**Solutions:**

* Sharding
* Hash-based partitioning

### Problem 2: High Read Traffic

**Solutions:**

* Caching (Redis)
* Read replicas

### Problem 3: ID Generation at Scale

**Solutions:**

* Snowflake ID generation
* Pre-generated ID blocks

---

##  Bottlenecks & Fixes

| Issue       | Solution           |
| ----------- | ------------------ |
| Hot URLs    | Cache              |
| DB overload | Sharding           |
| Collisions  | Unique ID strategy |
| Latency     | CDN + Cache        |

---

## Advanced Considerations

* Custom aliases (e.g., bit.ly/custom)
* Rate limiting (prevent abuse)
* Analytics (click tracking)
* Malware / spam detection

---

##  Mental Model

```
Short URL = Primary Key
Database = Lookup Table
Cache = Speed Layer
```

---


> This is a read-heavy system, so prioritize caching and fast lookups over write optimization.

* Use Base62 encoding for short URLs
* Optimize reads using caching
* Scale using sharding and replication
* Focus on availability and latency

---
