# AWS Data Wrangler

## aws s3 ls

```
import os
os.environ["AWS_DEFAULT_PROFILE"] = "personal"
os.environ["AWS_DEFAULT_REGION"] = "us-east-1"

# Enter your bucket name
import getpass
bucket = getpass.getpass()
path = getpass.getpass()
s3_path = f"s3://{bucket}/{path}"
wr.s3.list_objects(path=s3_path)
```

