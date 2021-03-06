This data directory was created from beta-20160414 (with a small modification
to disable the Raft log queue) and the following shell script:

```shell
rm -rf cockroach-data
./cockroach start --alsologtostderr=INFO&
sleep 1
./cockroach debug range split key_7429_a
./cockroach debug range split key_7429_b
sleep 1;
./cockroach debug kv scan;
killall -9 cockroach
```

Its purpose is to create an initial state which is vulnerable to
https://github.com/cockroachdb/cockroach/issues/7389 when opened from versions
after https://github.com/cockroachdb/cockroach/pull/7310
(beta-20160609-346-g5f78d71). The special feature of this state is that it
contains ranges for which no RaftTruncatedState has been persisted. See
https://github.com/cockroachdb/cockroach/pull/7429 for the PR which introduces
the corresponding migration.
