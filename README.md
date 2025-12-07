### Benchmark Results

The benchmark was executed using a synthetic CSV dataset containing 10,000 rows and 5 columns.  
Each test was run 3 times and the average time was recorded.

| Operation                    | Average Time (s) |
|-----------------------------|------------------|
| `csv.writer`                | 0.025959         |
| `CustomCsvWriter`           | 0.026989         |
| `csv.reader`                | 0.029148         |
| `CustomCsvReader`           | 0.219198         |
