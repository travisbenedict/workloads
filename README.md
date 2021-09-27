Benchmark-Workloads
------------

This repository contains the default workload specifications for the OpenSearch benchmarking tool [Benchmark](https://github.com/opensearch-project/OpenSearch-Benchmark/blob/main/DEVELOPER_GUIDE.md).

Workloads are used to describe benchmarks in Benchmark.

You should not need to use this repository directly, except if you want to look under the hood or create your own workloads. We have created a [tutorial on how to create your own workloads](https://github.com/opensearch-project/OpenSearch-Benchmark/blob/main/DEVELOPER_GUIDE.md).

Versioning Scheme
-----------------

Refer to the official [Benchmark docs](https://github.com/opensearch-project/OpenSearch-Benchmark/blob/main/DEVELOPER_GUIDE.md) for more details.

How to Contribute
-----------------

If you want to contribute a workload, please ensure that it works against the master version of OpenSearch (i.e. submit PRs against the master branch). We can then check whether it's feasible to backport the workload to earlier OpenSearch versions.

See all details in the [contributor guidelines](https://github.com/opensearch-project/OpenSearch-Benchmark/blob/main/CONTRIBUTING.md).

Backporting changes
-------------------

If you are a contributor with direct commit access to this repository then please backport your changes. This ensures that workloads do not work only for the latest `master` version of OpenSearch but also for older versions. Apply backports with cherr-picks. Below you can find a walkthrough:

Assume we've pushed commit `a7e0937` to master and want to backport it. This is a change to the `noaa` workload. Let's check what branches are available for backporting:

```
daniel@io:workloads/default ‹master›$ git branch -r
  origin/1
  origin/2
  origin/5
  origin/HEAD -> origin/master
  origin/master
```

We'll go backwards starting from branch `5`, then branch `2` and finally branch `1`. After applying a change, we will test whether the workload works as is for an older version of OpenSearch.

```
git checkout 5
git cherry-pick a7e0937

# test the change now with an OpenSearch 5.x distribution
osbenchmark execute_test --workload=noaa --distribution-version=5.4.3 --test-mode

# push the change
git push origin 5
```

This particular workload uses features that are only available in OpenSearch 5 and later so we will stop here but the process continues until we've reached the earliest branch.

Sometimes it is necessary to remove individual operations from a workload that are not supported by earlier versions. This graceful fallback is a compromise to allow to run a subset of the workload on older versions of OpenSearch too. If this is necessary then it's best to do these changes in a separate commit. Also, don't forget to cherry-pick this separate commit too to even earlier versions if necessary.


License
-------

There is no single license for this repository. Licenses are chosen per workload. They are typically licensed under the same terms as the source data. See the README files of each workload for more details.