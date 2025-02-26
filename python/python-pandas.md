# Python Pandas

## Convert JSON to CSV

```
import pandas as pd
df = pd.read_json('results.json')
df.to_csv('results.csv', index=None)
```
