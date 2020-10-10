# AWS Athena Queries

Athena query on top of CloudTracker to get the resource ARN. We can use this to suggest a least privilege IAM policy.

```text
select (eventsource, eventname, resources) from cloudtrail_logs_000000000000 where (userIdentity.sessionContext.sessionIssuer.arn = 'arn:aws:iam::000000000000:role/myrole') and (((year = '2020' and month = '07') or (year = '2020' and month = '08')) and errorcode IS NULL) limit 10;
```

