# Batching
```
var numberOfRecords = 1000;
var batchSize =10;
for(i = 0; i < numberOfRecords; i+ = batchSize)
{
  console.WriteLine($"processing records from {i+1} to {i + batch});
  LoadPage(i, batchSize)
}
LoadPage(int from, int to)
{
  return records.Skip(i).Take(batchSize);
}
```
### UseCases
1.If you are grouping in batch, the records may not group propertly
  -- may be the key exist in two different batches.

