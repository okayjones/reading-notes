# Hash Tables

[Source: Hashtable](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-30/resources/Hashtables.html)

- `Hash` Result of an algo converting to a value, used to determine index of the array.
- `Buckets` What is contained in each index. If collision occurs, an index (bucket) may contain multiple key/value pairs.
- `Collision` When more than one key gets hashed to same index

## Structure

- Every Bucket (or Node) has both a key and value
- Store the key in the data structure, and quickly get the value
- Use a hash to identify which bucket a key belongs in
- Allows O(1) lookup, since we can identify the bucket

## Creating the Hash

Function that turns a key into an integer. Must be deterministic (output determined by input). A key should always return the same hash. Traditionally arrays are used. Size of the array is important because it will determine index placement.

Example hash

```bash
Key = 'Cat'
Value = 'Josie'

# Add or multiply ASCII values together
67 + 97 + 116
# Multiple by prime number
280 * 599 = 69648
# Use modulo to get remainer, divide by array size
69648 % 1024 = 16
# Key places in index 16
```

## Collisions

Multiple keys can map to the same index. We can solve this by initializing a LinkedList in each index. Then, we'll add to the LinkedList when collisions occur.

When searching in a specific index, you traverse the LinkedList to find the key.

```javascript
hashMap.Add("Pioneer Square", 98104);
hashMap.Add("Alki Beach", 98116);
```

```bash
Bucket 92: [{Pioneer Square: 98104} --> {Alki Beach: 98116}]
```

> Important to store key with value so you can identify it in the list.

## Methods

- `Add()` - Send key to GetHash, go to index. If value doesn't exist, add it. If value does exist, add to data structure in the bucket.
- `Find()` - Takes a key, uses GetHash, goes to index. Iterate through and return value.
- `Contains()` - Accept a key, return boolean if it exists. Call GetHas, check if key exists in index.
- `GetHash()` - Accept key, conduct hash, return index.
