### Dive
Check what OCI images has without running them:
`https://github.com/wagoodman/dive`
Usage from local docker registry (indico example):
```
dive localhost:32000/indico:latest
```

### Act
To run github actions locally for testing:
https://github.com/nektos/act

### Spread
https://github.com/canonical/spread
"Because our integration test machinery was unreasonably frustrating. It was slow, very unstable, hard to make sense of the output, impossible to debug, hard to write tests for, hard to run on multiple environments, and parallelism was not a thing.

Spread came out as a plesant way to fix that. A few simple and concrete concepts that are fun to play with and fix the exact piece missing in the puzzle. It's not Jenkins, it's not Travis, it's not a library, not a language, and it's not even specific to testing. It's a simple way to express what to run and where, what to do before and after it runs, and how to duplicate jobs with minor variations without copy & paste."

