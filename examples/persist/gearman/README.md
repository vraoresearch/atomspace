# Minimal Gearman Example

## Description

Gearman example to run a simple task in parallel. The task in question
is `do-work` in file `task.scm`, some simple crushing number that
takes about a second to complete.

The goal is to use gearman to run 10 instances of this task in
parallel across 3 workers.

## Usage

Launch `worker-1.scm` to `worker-3.scm` in three terminals (using
option -l of `guile`).

Then launch `client.scm` in a fourth terminal. In should crash with
message

```
_client_do(GEARMAN_ERRNO) occurred during gearman_client_run_ta_client_do(GEARMAN_ERRNO) occurred during gearman_client_run_tasks() -> libgearman/client.cc:212 .cc:171_client_do(GEARMAN_ERRNO) occurred during gearman_client_run_tasks() -> libgearman/client.cc:212_client_do(GEARMAN_ERRNO) occurred during gearman_client_run_tasks() -> libgearman/client.cc:212Segmentation fault (core dumped)
```

If you replace `par-map` by `map` in `client.scm` then it doesn't
crash, but doesn't execute the tasks in parallel either.

The reason `par-map` is used is because `dist-eval` is blocking. I
don't see another way to use more than one worker at a time.
