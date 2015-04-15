# Benchmark of Swift extensions compilation times

This benchmark compares compilation time of class with N method vs. time to compile class and N single-method extensions in Swift.

## Usage

```
rake benchmark
```

## Results

On my machine it produces the following results:

| n | methods | extensions |
| ---: | ---: | ---: |
|100 | 0,15 | 0,15 |
|1000 | 1,05 | 1,1067 |
|2000 | 2,0933 | 2,4467 |
|3000 | 3,2867 | 4,2 |
|5000 | 6,033 | 8,6367 |
|10000| 17,4767 | 33,21 |
