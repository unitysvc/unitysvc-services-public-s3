# UnitySVC Services - Public S3 Datasets

This repository hosts service data for free public S3 datasets on the UnitySVC platform, provided by **UnitySVC Labs**.

## Overview

These services proxy publicly available S3 datasets (AWS Open Data Registry, Common Crawl, etc.) through the UnitySVC S3 storage gateway (`s3.unitysvc.com`). They serve triple duty:

- **Testing** the S3 gateway end-to-end with real data
- **Demonstrating** the platform to prospective customers and sellers
- **Providing real value** — curated access to popular datasets via a single API key

No files are self-hosted. The gateway proxies to existing public S3 buckets.

## Services

| Service | Upstream Bucket | Description |
|---|---|---|
| `open-weather` | `s3://noaa-ghcn-pds/` | NOAA daily climate records from 100K+ stations worldwide |
| `open-imagery` | `s3://spacenet-dataset/` | SpaceNet satellite imagery with labeled features |
| `open-crawl` | `s3://commoncrawl/` | Common Crawl web archive (petabytes of web data) |

All services are free (price: $0).

## Repository Structure

```
data/
└── unitysvc-labs/
    ├── provider.toml          # UnitySVC Labs provider metadata
    ├── docs/                  # Shared documentation and code examples
    │   ├── description.md
    │   ├── code-example.py.j2
    │   └── code-example.sh.j2
    └── services/
        ├── open-weather/      # NOAA weather data
        │   ├── offering.json
        │   └── listing.json
        ├── open-imagery/      # SpaceNet satellite imagery
        │   ├── offering.json
        │   └── listing.json
        └── open-crawl/        # Common Crawl web archive
            ├── offering.json
            └── listing.json
```

## Usage

After enrolling in a service, use any S3-compatible client:

```python
import boto3

s3 = boto3.client('s3',
    endpoint_url='https://s3.unitysvc.com',
    aws_access_key_id='svcpass_your_api_key',
    aws_secret_access_key='not-used',
)

# Browse NOAA weather data
response = s3.list_objects_v2(Bucket='open-weather', MaxKeys=10)
for obj in response.get('Contents', []):
    print(f"{obj['Key']}  ({obj['Size']} bytes)")
```

## Related

- [#531](https://github.com/unitysvc/unitysvc/issues/531) — S3-compatible storage gateway
- [#576](https://github.com/unitysvc/unitysvc/issues/576) — Free S3 gateway demo services
- [unitysvc-services](https://github.com/unitysvc/unitysvc-services) — Seller SDK
