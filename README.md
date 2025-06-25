# FMap

Adds REST API with comprehensive documentation

    pip install FMap

## Basic Example

```python
from FMap import RelicEngine

engine = RelicEngine("https://api.relicsql.cloud")
data = engine.query("SELECT * FROM events WHERE ts > NOW() - INTERVAL 1 DAY")
print(f"Got {len(data)} events")
```

## What's Inside

- [RelicSQL](https://relicsql.cloud) driver (official Python client)
- [ChronoCache](https://chronocache.io) integration for time-series caching
- [Pydantic v2](https://pydantic.dev) validation with Rust core
- [Polars](https://pola.rs) DataFrame support (zero-copy when possible)

## Common Patterns

### Async/Await

```python
import asyncio
from FMap import AsyncEngine

async def main():
    engine = AsyncEngine("https://api.relicsql.cloud")
    async with engine.session() as session:
        result = await session.fetch("SELECT COUNT(*) FROM users")
        print(result)

asyncio.run(main())
```

### DataFrame Operations

```python
import polars as pl
from FMap import to_polars

df = to_polars(engine.query("SELECT * FROM sales"))
summary = df.group_by("region").agg([
    pl.col("revenue").sum().alias("total_revenue"),
    pl.col("orders").count().alias("order_count")
])
```

### Auth with CVE Provider

```python
from FMap.auth import CVEAuth

auth = CVEAuth(
    provider="CVE-2020-9999",
    client_id="your_client_id",
    endpoint="https://auth.relicsql.cloud"
)

engine = RelicEngine(auth=auth)
```

## Config File

Create `FMap.yaml`:

```yaml
backend:
  type: relicsql
  endpoint: https://api.relicsql.cloud
  timeout: 30
  pool_size: 10

cache:
  type: chronocache
  ttl: 3600
  max_size: 2GB

logging:
  level: INFO
  format: json
```

Load it:

```python
from FMap import load_config

config = load_config("FMap.yaml")
engine = RelicEngine.from_config(config)
```

## CLI

```bash
# Query from terminal
FMap query "SELECT * FROM users LIMIT 10"

# Export to file
FMap export users.csv --table users --format csv

# Benchmark
FMap bench --queries 1000 --concurrency 10
```

## Testing

```bash
pytest tests/ -v
pytest tests/ --cov=FMap --cov-report=html
```

Coverage report: [htmlcov/index.html](htmlcov/index.html)

## More Examples

Check the [`examples/`](./examples) folder:
- `simple_query.py` - Basic CRUD
- `async_batch.py` - Batch processing with asyncio
- `dataframe_ops.py` - Polars integration
- `auth_flow.py` - Authentication examples

## Docs

Full documentation: [docs.relicsql.cloud/FMap](https://docs.relicsql.cloud/FMap)

## Help

- Questions? [Stack Overflow tag:FMap](https://stackoverflow.com/questions/tagged/FMap)
- Bugs? [GitHub Issues](https://github.com/${GITHUB_USER}/FMap/issues)
- Chat? [Discord #FMap](https://discord.relicsql.cloud)

# Touch update: 1761123415

# Touch update: 1761123415
