# Boto3 Client Example

This pattern keeps configuration at the boundary, uses the standard boto3 credential chain, sets bounded timeouts, and translates expected failures without hiding them. In production, add metrics and a request correlation ID around the call.

```python
import os
from typing import Any

import boto3
from botocore.config import Config
from botocore.exceptions import ClientError, ConnectTimeoutError, ReadTimeoutError


def get_artifact_metadata() -> dict[str, Any] | None:
    client = boto3.client(
        "s3",
        region_name=os.environ["AWS_REGION"],
        config=Config(
            connect_timeout=3,
            read_timeout=10,
            retries={"mode": "standard", "max_attempts": 3},
        ),
    )

    try:
        response = client.head_object(
            Bucket=os.environ["ARTIFACT_BUCKET"],
            Key="reports/latest.json",
        )
    except (ConnectTimeoutError, ReadTimeoutError):
        raise RuntimeError("S3 metadata lookup timed out")
    except ClientError as error:
        code = error.response.get("Error", {}).get("Code")
        if code in {"404", "NoSuchKey", "NotFound"}:
            return None
        raise

    return {
        "content_length": response.get("ContentLength", 0),
        "etag": response.get("ETag"),
        "last_modified": response.get("LastModified"),
    }
```

The example does not put access keys in source code. The runtime role should be limited to `s3:HeadObject` on the required bucket prefix. Unit tests should inject or stub the client and cover both not-found and access-denied responses.
