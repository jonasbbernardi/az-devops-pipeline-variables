
# Pipeline counter variable proof of concept

## Introduction

Using Azure DevOps pipelines to validate a counter as variable exported between stages with is using the deployment job.

```yml
stage1:
  var1: counter
stage2:
  var2: stage1.var1
  # usage of var2
```

The counter `counter` must be increased each "run" of stage1, stage2 must always catch the last value of the counter.

## Tests

To validate the solution:

Run pipelines in sequency:

1. Complete
2. Just stage1
3. Just stage2
4. Just stage2
5. stage1 + stage2
6. Just stage1
7. stage1 + stage2

Results in format `> run (attempt, counter)`

```bash
# 1. Complete
> build (1) > stage1 (1, 1) > stage2 (1, 1)
# 2. Just stage1
> build (1) > stage1 (2, 2) > stage2 (1, 1)
# 3. Just stage2
> build (1) > stage1 (2, 2) > stage2 (2, 2)
# 4. Just stage2
> build (1) > stage1 (2, 2) > stage2 (3, 2)
# 5. stage1 + stage2
> build (1) > stage1 (3, 3) > stage2 (4, 3)
# 6. Just stage1
> build (1) > stage1 (4, 4) > stage2 (4, 3)
# 7. stage1 + stage2
> build (1) > stage1 (5, 5) > stage2 (5, 5)
```
