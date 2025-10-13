### Elasticsearch Demo Instructions

---

#### 1. **Start Elasticsearch Locally**

```bash
docker run -d \
  --name elasticsearch \
  -p 9200:9200 \
  -e "discovery.type=single-node" \
  -e "xpack.security.enabled=false" \
  docker.elastic.co/elasticsearch/elasticsearch:8.15.0
```

---

#### 2. **Add Elasticsearch MCP (STDIO) to Claude Desktop**

Edit the config file:

`~/Library/Application Support/Claude/claude_desktop_config.json`

```json
{
  "elasticsearch-mcp-server": {
    "command": "docker",
    "args": [
      "run", "-i", "--rm",
      "-e", "ES_URL",
      "docker.elastic.co/mcp/elasticsearch",
      "stdio"
    ],
    "env": {
      "ES_URL": "http://host.docker.internal:9200"
    }
  }
}
```

---

#### 3. **Create a Products Index**

```bash
curl -X PUT "localhost:9200/products?pretty"
```

Expected response:

```json
{
  "acknowledged": true,
  "shards_acknowledged": true,
  "index": "products"
}
```

---

#### 4. **Add Product Data**

```bash
curl -X POST "localhost:9200/products/_bulk?pretty" \
  -H "Content-Type: application/json" \
  -d '
{ "index": { "_id": 14 } }
{ "name": "The Infinite Coffee Mug", "price": 5.99, "category": "Coffee" }
{ "index": { "_id": 15 } }
{ "name": "Self-Watering Plant for People Who Kill Plants", "price": 29.99, "category": "Home" }
{ "index": { "_id": 16 } }
{ "name": "AI-Powered Sock Organizer", "price": 49.99, "category": "Gadgets" }
{ "index": { "_id": 17 } }
{ "name": "Invisible Umbrella", "price": 19.99, "category": "Accessories" }
{ "index": { "_id": 18 } }
{ "name": "Shower Beer Holder", "price": 12.99, "category": "Bathroom" }
{ "index": { "_id": 19 } }
{ "name": "Pet Rock 2.0 (With Bluetooth)", "price": 9.99, "category": "Pets" }
{ "index": { "_id": 20 } }
{ "name": "Time Travel Coffee Mug (Slightly Used)", "price": 199.99, "category": "Coffee" }
{ "index": { "_id": 21 } }
{ "name": "Anti-Snore Pillow (Whisper Mode)", "price": 39.99, "category": "Home" }
{ "index": { "_id": 22 } }
{ "name": "Portable Hole (Like in the Cartoons)", "price": 99.99, "category": "Gadgets" }
{ "index": { "_id": 23 } }
{ "name": "Mind-Controlled Pizza Cutter", "price": 79.99, "category": "Kitchen" }
'
```
You can inspect them using:
```bash
curl -X GET "localhost:9200/products/_search?pretty" \
  -H "Content-Type: application/json" \
  -d '
{
  "query": {
    "match_all": {}
  }
}
'

```

---

#### 5. **Query Example (via Claude)**

```bash
gimme products whose price falls in the range of 15-50 and which are fun buys
```

> LLM uses judgment to interpret 'fun buys'.

---

#### 6. **Add Log Data**

```bash
curl -X POST "http://localhost:9200/_bulk" \
  -H "Content-Type: application/json" \
  --data-binary "@assets/logs.json"
```

Check uploaded logs:
```bash
curl -X GET "localhost:9200/app-logs/_search?pretty" \
  -H "Content-Type: application/json" \
  -d '
{
  "query": {
    "match_all": {}
  }
}
'

```

Prompt example:

```bash
Check app logs in ES. We're not seeing fresh data. Why?
```

---

#### Bonus. **Start MCP Inspector and use to inspect!**

```bash
npx @modelcontextprotocol/inspector@latest
```

---
