---
description: JIRA Query Language commands.
---

# JIRA

### JQL Queries for Filters <a id="filter-jql-queries"></a>

Me, Open - Mentions and Assignments:

```text
project = PROJ AND status = Open AND (assignee in (currentUser()) OR summary ~ currentUser() OR description ~ currentUser() OR comment ~ currentUser()) ORDER BY updated DESC
```

Me, Open - as Reporter:

```text
project = PROJ AND status = Open AND reporter in (currentUser()) ORDER BY updated DESC
```

Me, Open - Assigned:

```text
project = PROJ AND status = Open AND assignee in (currentUser()) ORDER BY updated DESC
```

Me, All - Mentions/Assignments:

```text
project = PROJ AND (assignee in (currentUser()) OR summary ~ currentUser() OR description ~ currentUser() OR comment ~ currentUser()) ORDER BY updated DESC
```

Me, Open Assigned Last Week:

```text
project = PROJ AND status = Open AND created >= -1w AND assignee in (currentUser()) ORDER BY updated DESC
```

Me, Open Overdue, Ordered by CreationDate:

```text
project = PROJ AND status = Open AND created <= -1w AND assignee in (currentUser()) ORDER BY createdDate
```



