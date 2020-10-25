# sentry-playground

Here is a playground to install self-hosted sentry in test environment

## Documentation

- [repository](https://github.com/getsentry/onpremise)
- [self-hosted](https://develop.sentry.dev/self-hosted/)
- [Improve Your Workflow with Sentry](https://blog.sentry.io/2018/03/06/the-sentry-workflow)

## Requirements

The host OS for playground was Ubuntu 18.04

Also there must be installed:
- `vagrant` for test environment
- `make` for automate some actions
- `python-venv` to install some dependencies w/o affecting host OS

System requirements:

Sentry self-hosted installation recommends minimun 2400Mb RAM. So vagrant enviromnent provides instans with 4 virtual CPUs and 4Gb of RAM.

## Environment

Install venv
```shell
make install_venv
```

Install ansible required roles
```shell
make ansible_requirements
```

Provide vagrant environment with docker installed
```shell
make vagrant_up
```

## Sentry installation

Login to vagrant instance
```shell
vagrant ssh
```

Download and extract latest release (TODO: install wget with ansible)
```shell
wget https://github.com/getsentry/onpremise/archive/20.10.1.tar.gz
tar xvf 20.10.1.tar.gz
cd onpremise-20.10.1/
```

Run `./install.sh` which takes care of all the things you need to get started, including a base-line configuration.
NOTE: It's required to set admin's email and password during the installation process.
```shell
./install.sh
```
<details><summary>output</summary>
<p>

```log
Checking minimum requirements...

Creating volumes for persistent storage...
Created sentry-data.
Created sentry-postgres.
Created sentry-redis.
Created sentry-zookeeper.
Created sentry-kafka.
Created sentry-clickhouse.
Created sentry-symbolicator.

Creating sentry/sentry.conf.py...
Creating sentry/config.yml...
Creating sentry/requirements.txt...
Creating symbolicator/config.yml...
Creating relay/config.yml...

Generating secret key...
Secret key written to sentry/config.yml

Fetching and updating Docker images...


ERROR: for zookeeper  Get https://registry-1.docker.io/v2/confluentinc/cp-zookeeper/manifests/5.5.0: EOF
Some service image(s) must be built from source by running:
    docker-compose build cron post-process-forwarder snuba-cleanup symbolicator-cleanup ingest-consumer web worker sentry-cleanup
Get https://registry-1.docker.io/v2/confluentinc/cp-zookeeper/manifests/5.5.0: EOF
20.10.1: Pulling from getsentry/sentry
8d691f585fa8: Pulling fs layer
3fd6980f9df6: Pulling fs layer
c4f890363b98: Pulling fs layer
b8cfb9853b0a: Pulling fs layer
03a65e68bd5f: Pulling fs layer
74fd7792bf9a: Pulling fs layer
bb2024cd6bbc: Pulling fs layer
e74fa5de680f: Pulling fs layer
dae0eea108a7: Pulling fs layer
07cc196e1a97: Pulling fs layer
1881e2b0218c: Pulling fs layer
0b6965ef0fc9: Pulling fs layer
5df780d49856: Pulling fs layer
ac2e6fded9c0: Pulling fs layer
0b6965ef0fc9: Waiting
5df780d49856: Waiting
ac2e6fded9c0: Waiting
bb2024cd6bbc: Waiting
e74fa5de680f: Waiting
b8cfb9853b0a: Waiting
03a65e68bd5f: Waiting
74fd7792bf9a: Waiting
07cc196e1a97: Waiting
1881e2b0218c: Waiting
dae0eea108a7: Waiting
3fd6980f9df6: Verifying Checksum
3fd6980f9df6: Download complete
b8cfb9853b0a: Verifying Checksum
b8cfb9853b0a: Download complete
03a65e68bd5f: Verifying Checksum
03a65e68bd5f: Download complete
c4f890363b98: Verifying Checksum
c4f890363b98: Download complete
74fd7792bf9a: Verifying Checksum
74fd7792bf9a: Download complete
bb2024cd6bbc: Download complete
8d691f585fa8: Verifying Checksum
8d691f585fa8: Pull complete
3fd6980f9df6: Pull complete
c4f890363b98: Pull complete
b8cfb9853b0a: Pull complete
03a65e68bd5f: Pull complete
74fd7792bf9a: Pull complete
bb2024cd6bbc: Pull complete
dae0eea108a7: Verifying Checksum
dae0eea108a7: Download complete
1881e2b0218c: Verifying Checksum
1881e2b0218c: Download complete
0b6965ef0fc9: Verifying Checksum
0b6965ef0fc9: Download complete
07cc196e1a97: Verifying Checksum
07cc196e1a97: Download complete
5df780d49856: Verifying Checksum
5df780d49856: Download complete
ac2e6fded9c0: Verifying Checksum
ac2e6fded9c0: Download complete
e74fa5de680f: Verifying Checksum
e74fa5de680f: Download complete
e74fa5de680f: Pull complete
dae0eea108a7: Pull complete
07cc196e1a97: Pull complete
1881e2b0218c: Pull complete
0b6965ef0fc9: Pull complete
5df780d49856: Pull complete
ac2e6fded9c0: Pull complete
Digest: sha256:c7f37d609b43fdafcb643655ab0d48c21d503e2b390928a5db67253bee4781e2
Status: Downloaded newer image for getsentry/sentry:20.10.1
docker.io/getsentry/sentry:20.10.1

Building and tagging Docker images...

Building web
Step 1/5 : ARG SENTRY_IMAGE
Step 2/5 : ARG SENTRY_PYTHON3
Step 3/5 : FROM ${SENTRY_IMAGE}${SENTRY_PYTHON3:+-py3}
 ---> b81fbb0a7446
Step 4/5 : COPY . /usr/src/sentry
 ---> 29d48499d607
Step 5/5 : RUN if [ -s /usr/src/sentry/requirements.txt ]; then pip install -r /usr/src/sentry/requirements.txt; fi
 ---> Running in 7c9ef82a9e03
/usr/local/lib/python2.7/site-packages/OpenSSL/crypto.py:12: CryptographyDeprecationWarning: Python 2 is no longer supported by the Python core team. Support for it is now deprecated in cryptography, and will be removed in a future release.
  from cryptography import x509
DEPRECATION: Python 2.7 will reach the end of its life on January 1st, 2020. Please upgrade your Python as Python 2.7 won't be maintained after that date. A future version of pip will drop support for Python 2.7. More details about Python 2 support in pip, can be found at https://pip.pypa.io/en/latest/development/release-process/#python-2-support
Removing intermediate container 7c9ef82a9e03
 ---> 309487808988

Successfully built 309487808988
Successfully tagged sentry-onpremise-local:latest
smtp uses an image, skipping
memcached uses an image, skipping
redis uses an image, skipping
postgres uses an image, skipping
zookeeper uses an image, skipping
kafka uses an image, skipping
clickhouse uses an image, skipping
snuba-api uses an image, skipping
snuba-consumer uses an image, skipping
snuba-outcomes-consumer uses an image, skipping
snuba-sessions-consumer uses an image, skipping
snuba-transactions-consumer uses an image, skipping
snuba-replacer uses an image, skipping
symbolicator uses an image, skipping
relay uses an image, skipping
nginx uses an image, skipping
Building snuba-cleanup          ... 
Building symbolicator-cleanup   ... 
Building web                    ... 
Building cron                   ... 
Building worker                 ... 
Building ingest-consumer        ... 
Building post-process-forwarder ... 
Building sentry-cleanup         ... 
Building symbolicator-cleanup
Building ingest-consumer
Building sentry-cleanup
Building web
Building cron
Step 1/5 : ARG BASE_IMAGE
Step 2/5 : FROM ${BASE_IMAGE}
 ---> f91ead8550da
Step 3/5 : RUN apt-get update && apt-get install -y --no-install-recommends cron &&     rm -r /var/lib/apt/lists/*
Step 1/5 : ARG SENTRY_IMAGE
Step 2/5 : ARG SENTRY_PYTHON3
Step 3/5 : FROM ${SENTRY_IMAGE}${SENTRY_PYTHON3:+-py3}
 ---> b81fbb0a7446
Step 4/5 : COPY . /usr/src/sentry
 ---> Using cache
 ---> 29d48499d607
Step 5/5 : RUN if [ -s /usr/src/sentry/requirements.txt ]; then pip install -r /usr/src/sentry/requirements.txt; fi
Step 1/5 : ARG SENTRY_IMAGE
Step 2/5 : ARG SENTRY_PYTHON3
Step 3/5 : FROM ${SENTRY_IMAGE}${SENTRY_PYTHON3:+-py3}
 ---> b81fbb0a7446
Step 4/5 : COPY . /usr/src/sentry
 ---> Using cache
 ---> 29d48499d607
Step 5/5 : RUN if [ -s /usr/src/sentry/requirements.txt ]; then pip install -r /usr/src/sentry/requirements.txt; fi
 ---> Using cache
 ---> 309487808988

Successfully built 309487808988
Successfully tagged sentry-onpremise-local:latest
Building cron                   ... done
Building worker
 ---> Using cache
 ---> 309487808988

Successfully built 309487808988
Successfully tagged sentry-onpremise-local:latest
Building snuba-cleanup
Building ingest-consumer        ... done
Step 1/5 : ARG BASE_IMAGE
Step 2/5 : FROM ${BASE_IMAGE}
 ---> 309487808988
Step 3/5 : RUN apt-get update && apt-get install -y --no-install-recommends cron &&     rm -r /var/lib/apt/lists/*
Step 1/5 : ARG SENTRY_IMAGE
Step 2/5 : ARG SENTRY_PYTHON3
Step 3/5 : FROM ${SENTRY_IMAGE}${SENTRY_PYTHON3:+-py3}
 ---> b81fbb0a7446
Step 4/5 : COPY . /usr/src/sentry
 ---> Using cache
 ---> 29d48499d607
Step 5/5 : RUN if [ -s /usr/src/sentry/requirements.txt ]; then pip install -r /usr/src/sentry/requirements.txt; fi
 ---> Using cache
 ---> 309487808988

Successfully built 309487808988
Successfully tagged sentry-onpremise-local:latest
Building web                    ... done
Building post-process-forwarder
 ---> Running in c0668efb12e7
 ---> Running in 140c9c22fe37
Step 1/5 : ARG SENTRY_IMAGE
Step 2/5 : ARG SENTRY_PYTHON3
Step 3/5 : FROM ${SENTRY_IMAGE}${SENTRY_PYTHON3:+-py3}
 ---> b81fbb0a7446
Step 4/5 : COPY . /usr/src/sentry
 ---> Using cache
 ---> 29d48499d607
Step 5/5 : RUN if [ -s /usr/src/sentry/requirements.txt ]; then pip install -r /usr/src/sentry/requirements.txt; fi
 ---> Using cache
 ---> 309487808988

Successfully built 309487808988
Successfully tagged sentry-onpremise-local:latest
Building worker                 ... done
Step 1/5 : ARG BASE_IMAGE
Step 2/5 : FROM ${BASE_IMAGE}
 ---> 1563b0e59ae1
Step 3/5 : RUN apt-get update && apt-get install -y --no-install-recommends cron &&     rm -r /var/lib/apt/lists/*
Step 1/5 : ARG SENTRY_IMAGE
Step 2/5 : ARG SENTRY_PYTHON3
Step 3/5 : FROM ${SENTRY_IMAGE}${SENTRY_PYTHON3:+-py3}
 ---> b81fbb0a7446
Step 4/5 : COPY . /usr/src/sentry
 ---> Using cache
 ---> 29d48499d607
Step 5/5 : RUN if [ -s /usr/src/sentry/requirements.txt ]; then pip install -r /usr/src/sentry/requirements.txt; fi
 ---> Running in 45c244971fb6
 ---> Using cache
 ---> 309487808988

Successfully built 309487808988
Successfully tagged sentry-onpremise-local:latest
Building post-process-forwarder ... done
Get:1 http://deb.debian.org/debian buster InRelease [121 kB]
Get:2 http://security.debian.org/debian-security buster/updates InRelease [65.4 kB]
Get:3 http://deb.debian.org/debian buster-updates InRelease [51.9 kB]
Get:1 http://security.debian.org/debian-security stretch/updates InRelease [53.0 kB]
Get:1 http://deb.debian.org/debian buster InRelease [121 kB]
Get:4 http://security.debian.org/debian-security buster/updates/main amd64 Packages [243 kB]
Ign:2 http://deb.debian.org/debian stretch InRelease
Get:2 http://security.debian.org/debian-security buster/updates InRelease [65.4 kB]
Get:3 http://deb.debian.org/debian stretch-updates InRelease [93.6 kB]
Get:5 http://deb.debian.org/debian buster/main amd64 Packages [7906 kB]
Get:3 http://deb.debian.org/debian buster-updates InRelease [51.9 kB]
Get:4 http://deb.debian.org/debian stretch Release [118 kB]
Get:5 http://security.debian.org/debian-security stretch/updates/main amd64 Packages [604 kB]
Get:6 http://deb.debian.org/debian stretch Release.gpg [2410 B]
Get:4 http://security.debian.org/debian-security buster/updates/main amd64 Packages [243 kB]
Get:7 http://deb.debian.org/debian stretch-updates/main amd64 Packages [2596 B]
Get:5 http://deb.debian.org/debian buster/main amd64 Packages [7906 kB]
Get:8 http://deb.debian.org/debian stretch/main amd64 Packages [7080 kB]
Get:6 http://deb.debian.org/debian buster-updates/main amd64 Packages [7856 B]
Fetched 8396 kB in 3s (3277 kB/s)
Reading package lists...
Reading package lists...
Building dependency tree...
Reading state information...
The following additional packages will be installed:
  lsb-base sensible-utils
Suggested packages:
  anacron logrotate checksecurity
Recommended packages:
  default-mta | mail-transport-agent
The following NEW packages will be installed:
  cron lsb-base sensible-utils
0 upgraded, 3 newly installed, 0 to remove and 17 not upgraded.
Need to get 143 kB of archives.
After this operation, 383 kB of additional disk space will be used.
Get:1 http://deb.debian.org/debian buster/main amd64 sensible-utils all 0.0.12 [15.8 kB]
Get:2 http://deb.debian.org/debian buster/main amd64 lsb-base all 10.2019051400 [28.4 kB]
Get:3 http://deb.debian.org/debian buster/main amd64 cron amd64 3.0pl1-134+deb10u1 [99.0 kB]
debconf: delaying package configuration, since apt-utils is not installed
Fetched 143 kB in 0s (581 kB/s)
Selecting previously unselected package sensible-utils.
(Reading database ... 11918 files and directories currently installed.)
Preparing to unpack .../sensible-utils_0.0.12_all.deb ...
Unpacking sensible-utils (0.0.12) ...
Selecting previously unselected package lsb-base.
Preparing to unpack .../lsb-base_10.2019051400_all.deb ...
Unpacking lsb-base (10.2019051400) ...
Selecting previously unselected package cron.
Preparing to unpack .../cron_3.0pl1-134+deb10u1_amd64.deb ...
Unpacking cron (3.0pl1-134+deb10u1) ...
Setting up lsb-base (10.2019051400) ...
Setting up sensible-utils (0.0.12) ...
Setting up cron (3.0pl1-134+deb10u1) ...
Adding group `crontab' (GID 101) ...
Done.
invoke-rc.d: could not determine current runlevel
invoke-rc.d: policy-rc.d denied execution of start.
Fetched 7953 kB in 4s (1892 kB/s)
Reading package lists...Removing intermediate container c0668efb12e7
 ---> 8bcfcb5b6ed2
Step 4/5 : COPY entrypoint.sh /entrypoint.sh
 ---> 16f0cee0b676
Step 5/5 : ENTRYPOINT ["/entrypoint.sh"]
 ---> Running in a0e30d029933

Removing intermediate container a0e30d029933
 ---> e4462dc7c20b

Successfully built e4462dc7c20b
Successfully tagged sentry-cleanup-onpremise-local:latest
Building sentry-cleanup         ... done
Reading package lists...Get:6 http://deb.debian.org/debian buster-updates/main amd64 Packages [7856 B]

Building dependency tree...
Reading state information...
Suggested packages:
  anacron logrotate checksecurity
Recommended packages:
  exim4 | postfix | mail-transport-agent
The following NEW packages will be installed:
  cron
0 upgraded, 1 newly installed, 0 to remove and 0 not upgraded.
Need to get 95.4 kB of archives.
After this operation, 257 kB of additional disk space will be used.
Get:1 http://deb.debian.org/debian stretch/main amd64 cron amd64 3.0pl1-128+deb9u1 [95.4 kB]
debconf: delaying package configuration, since apt-utils is not installed
Fetched 95.4 kB in 0s (505 kB/s)
Selecting previously unselected package cron.
(Reading database ... 6661 files and directories currently installed.)
Preparing to unpack .../cron_3.0pl1-128+deb9u1_amd64.deb ...
Unpacking cron (3.0pl1-128+deb9u1) ...
Setting up cron (3.0pl1-128+deb9u1) ...
Adding group `crontab' (GID 101) ...
Done.
update-rc.d: warning: start and stop actions are no longer supported; falling back to defaults
invoke-rc.d: could not determine current runlevel
invoke-rc.d: policy-rc.d denied execution of start.
Fetched 8396 kB in 6s (1459 kB/s)
Reading package lists...Removing intermediate container 140c9c22fe37
 ---> 037f23f37a21
Step 4/5 : COPY entrypoint.sh /entrypoint.sh
 ---> 8b1e936eda50
Step 5/5 : ENTRYPOINT ["/entrypoint.sh"]
 ---> Running in 428975a275b5
Removing intermediate container 428975a275b5
 ---> 9827e513f160

Successfully built 9827e513f160
Successfully tagged symbolicator-cleanup-onpremise-local:latest
Building symbolicator-cleanup   ... done

Reading package lists...
Building dependency tree...
Reading state information...
The following additional packages will be installed:
  lsb-base sensible-utils
Suggested packages:
  anacron logrotate checksecurity
Recommended packages:
  default-mta | mail-transport-agent
The following NEW packages will be installed:
  cron lsb-base sensible-utils
0 upgraded, 3 newly installed, 0 to remove and 1 not upgraded.
Need to get 143 kB of archives.
After this operation, 383 kB of additional disk space will be used.
Get:1 http://deb.debian.org/debian buster/main amd64 sensible-utils all 0.0.12 [15.8 kB]
Get:2 http://deb.debian.org/debian buster/main amd64 lsb-base all 10.2019051400 [28.4 kB]
Get:3 http://deb.debian.org/debian buster/main amd64 cron amd64 3.0pl1-134+deb10u1 [99.0 kB]
debconf: delaying package configuration, since apt-utils is not installed
Fetched 143 kB in 0s (513 kB/s)
Selecting previously unselected package sensible-utils.
(Reading database ... 6829 files and directories currently installed.)
Preparing to unpack .../sensible-utils_0.0.12_all.deb ...
Unpacking sensible-utils (0.0.12) ...
Selecting previously unselected package lsb-base.
Preparing to unpack .../lsb-base_10.2019051400_all.deb ...
Unpacking lsb-base (10.2019051400) ...
Selecting previously unselected package cron.
Preparing to unpack .../cron_3.0pl1-134+deb10u1_amd64.deb ...
Unpacking cron (3.0pl1-134+deb10u1) ...
Setting up lsb-base (10.2019051400) ...
Setting up sensible-utils (0.0.12) ...
Setting up cron (3.0pl1-134+deb10u1) ...
Adding group `crontab' (GID 101) ...
Done.
invoke-rc.d: could not determine current runlevel
invoke-rc.d: policy-rc.d denied execution of start.
Removing intermediate container 45c244971fb6
 ---> ce79ea902513
Step 4/5 : COPY entrypoint.sh /entrypoint.sh
 ---> f9b44418b875
Step 5/5 : ENTRYPOINT ["/entrypoint.sh"]
 ---> Running in ced5b24d747e
Removing intermediate container ced5b24d747e
 ---> 43e5fbcb483a

Successfully built 43e5fbcb483a
Successfully tagged snuba-cleanup-onpremise-local:latest
Building snuba-cleanup          ... done

Docker images built.
Removing network onpremise_default
Network onpremise_default not found.
Removing network sentry_onpremise_default
Network sentry_onpremise_default not found.
Creating network "sentry_onpremise_default" with the default driver
Creating volume "sentry_onpremise_sentry-secrets" with default driver
Creating volume "sentry_onpremise_sentry-smtp" with default driver
Creating volume "sentry_onpremise_sentry-zookeeper-log" with default driver
Creating volume "sentry_onpremise_sentry-kafka-log" with default driver
Creating volume "sentry_onpremise_sentry-smtp-log" with default driver
Creating volume "sentry_onpremise_sentry-clickhouse-log" with default driver
Pulling zookeeper (confluentinc/cp-zookeeper:5.5.0)...
./install.sh: line 233: [[: 5.5.0: Pulling from confluentinc/cp-zookeeper
Digest: sha256:6e33666a21ed552cf4a6b9096a2fa94c954a60c17ec470a20f0422b9cbaa6a26
Status: Downloaded newer image for confluentinc/cp-zookeeper:5.5.0
0: syntax error: invalid arithmetic operator (error token is ".5.0: Pulling from confluentinc/cp-zookeeper
Digest: sha256:6e33666a21ed552cf4a6b9096a2fa94c954a60c17ec470a20f0422b9cbaa6a26
Status: Downloaded newer image for confluentinc/cp-zookeeper:5.5.0
0")
Bootstrapping and migrating Snuba...
Creating sentry_onpremise_redis_1 ... 
Creating sentry_onpremise_clickhouse_1 ... 
Creating sentry_onpremise_zookeeper_1  ... 
Creating sentry_onpremise_clickhouse_1 ... done
Creating sentry_onpremise_zookeeper_1  ... done
Creating sentry_onpremise_kafka_1      ... 
Creating sentry_onpremise_redis_1      ... done
Creating sentry_onpremise_kafka_1      ... done
+ '[' b = - ']'
+ snuba bootstrap --help
+ set -- snuba bootstrap --no-migrate --force
+ set gosu snuba snuba bootstrap --no-migrate --force
+ exec gosu snuba snuba bootstrap --no-migrate --force
%3|1603629851.464|FAIL|rdkafka#producer-1| [thrd:kafka:9092/bootstrap]: kafka:9092/bootstrap: Connect to ipv4#172.18.0.5:9092 failed: Connection refused (after 2ms in state CONNECT)
%3|1603629852.463|FAIL|rdkafka#producer-1| [thrd:kafka:9092/bootstrap]: kafka:9092/bootstrap: Connect to ipv4#172.18.0.5:9092 failed: Connection refused (after 0ms in state CONNECT, 1 identical error(s) suppressed)
2020-10-25 12:44:12,463 Connection to Kafka failed (attempt 0)
Traceback (most recent call last):
  File "/usr/src/snuba/snuba/cli/bootstrap.py", line 58, in bootstrap
    client.list_topics(timeout=1)
cimpl.KafkaException: KafkaError{code=_TRANSPORT,val=-195,str="Failed to get metadata: Local: Broker transport failure"}
%3|1603629853.466|FAIL|rdkafka#producer-2| [thrd:kafka:9092/bootstrap]: kafka:9092/bootstrap: Connect to ipv4#172.18.0.5:9092 failed: Connection refused (after 0ms in state CONNECT)
2020-10-25 12:44:14,467 Connection to Kafka failed (attempt 1)
Traceback (most recent call last):
  File "/usr/src/snuba/snuba/cli/bootstrap.py", line 58, in bootstrap
    client.list_topics(timeout=1)
cimpl.KafkaException: KafkaError{code=_TRANSPORT,val=-195,str="Failed to get metadata: Local: Broker transport failure"}
2020-10-25 12:44:15,885 Topic cdc created
2020-10-25 12:44:15,885 Topic events created
2020-10-25 12:44:15,885 Topic errors-replacements created
2020-10-25 12:44:15,885 Topic event-replacements created
2020-10-25 12:44:15,885 Topic snuba-commit-log created
2020-10-25 12:44:15,885 Topic ingest-sessions created
2020-10-25 12:44:15,885 Topic outcomes created
Starting sentry_onpremise_clickhouse_1 ... 
Starting sentry_onpremise_clickhouse_1 ... done
Starting sentry_onpremise_zookeeper_1  ... 
Starting sentry_onpremise_redis_1      ... 
Starting sentry_onpremise_zookeeper_1  ... done
Starting sentry_onpremise_redis_1      ... done
Starting sentry_onpremise_kafka_1      ... 
Starting sentry_onpremise_kafka_1      ... done
+ '[' m = - ']'
+ snuba migrations --help
+ set -- snuba migrations migrate --force
+ set gosu snuba snuba migrations migrate --force
+ exec gosu snuba snuba migrations migrate --force
Finished running migrations

Creating additional Kafka topics...
Starting sentry_onpremise_zookeeper_1 ... 
Starting sentry_onpremise_zookeeper_1 ... done
Created topic ingest-attachments.

Creating additional Kafka topics...
Starting sentry_onpremise_zookeeper_1 ... 
Starting sentry_onpremise_zookeeper_1 ... done
Created topic ingest-transactions.

Creating additional Kafka topics...
Starting sentry_onpremise_zookeeper_1 ... 
Starting sentry_onpremise_zookeeper_1 ... done
Created topic ingest-events.


Setting up database...
Creating sentry_onpremise_smtp_1 ... 
Starting sentry_onpremise_clickhouse_1 ... 
Creating sentry_onpremise_memcached_1  ... 
Starting sentry_onpremise_clickhouse_1 ... done
Starting sentry_onpremise_redis_1      ... 
Starting sentry_onpremise_redis_1      ... done
Creating sentry_onpremise_symbolicator_1 ... 
Creating sentry_onpremise_postgres_1     ... 
Starting sentry_onpremise_zookeeper_1    ... 
Starting sentry_onpremise_zookeeper_1    ... done
Starting sentry_onpremise_kafka_1        ... 
Starting sentry_onpremise_kafka_1        ... done
Creating sentry_onpremise_snuba-outcomes-consumer_1 ... 
Creating sentry_onpremise_snuba-transactions-consumer_1 ... 
Creating sentry_onpremise_snuba-api_1                   ... 
Creating sentry_onpremise_snuba-sessions-consumer_1     ... 
Creating sentry_onpremise_snuba-replacer_1              ... 
Creating sentry_onpremise_snuba-consumer_1              ... 
Creating sentry_onpremise_smtp_1                        ... done
Creating sentry_onpremise_memcached_1                   ... done
Creating sentry_onpremise_postgres_1                    ... done
Creating sentry_onpremise_symbolicator_1                ... done
Creating sentry_onpremise_snuba-api_1                   ... done
Creating sentry_onpremise_snuba-transactions-consumer_1 ... done
Creating sentry_onpremise_snuba-consumer_1              ... done
Creating sentry_onpremise_snuba-outcomes-consumer_1     ... done
Creating sentry_onpremise_snuba-sessions-consumer_1     ... done
Creating sentry_onpremise_snuba-replacer_1              ... done
12:44:35 [WARNING] sentry.utils.geo: settings.GEOIP_PATH_MMDB not configured.
/usr/local/lib/python2.7/site-packages/OpenSSL/crypto.py:12: CryptographyDeprecationWarning: Python 2 is no longer supported by the Python core team. Support for it is now deprecated in cryptography, and will be removed in a future release.
  from cryptography import x509
12:44:37 [INFO] sentry.plugins.github: apps-not-configured
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, jira_ac, nodestore, sentry, sessions, sites, social_auth
Running migrations:
  Applying sentry.0001_initial... OK
  Applying contenttypes.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0001_initial... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying auth.0008_alter_user_username_max_length... OK
  Applying jira_ac.0001_initial... OK
  Applying nodestore.0001_initial... OK
  Applying sentry.0002_912_to_recent... OK
  Applying sentry.0003_auto_20191022_0122... OK
  Applying sentry.0004_bitfieldtestmodel_blankjsonfieldtestmodel_callabledefaultmodel_jsonfieldtestmodel_jsonfieldwithdefau... OK
  Applying sentry.0005_fix_content_types... OK
  Applying sentry.0006_sentryapp_date_published... OK
  Applying sentry.0007_auto_20191029_0131... OK
  Applying sentry.0008_auto_20191030_0016... OK
  Applying sentry.0009_auto_20191101_1608... OK
  Applying sentry.0010_auto_20191104_1641... OK
  Applying sentry.0011_remove_pagerdutyservice_service_id_from_state... OK
  Applying sentry.0012_remove_pagerdutyservice_service_id... OK
  Applying sentry.0013_auto_20191111_1829... OK
  Applying sentry.0014_delete_sentryappwebhookerror... OK
  Applying sentry.0015_delete_sentryappwebhookerror_db... OK
  Applying sentry.0016_delete_alert_rule_deprecated_fields... OK
  Applying sentry.0017_incident_aggregation... OK
  Applying sentry.0018_discoversavedquery_version... OK
  Applying sentry.0019_auto_20191114_2040... OK
  Applying sentry.0020_auto_20191125_1420... OK
  Applying sentry.0021_auto_20191203_1803... OK
  Applying sentry.0021_auto_20191202_1716... OK
  Applying sentry.0022_merge... OK
  Applying sentry.0023_hide_environment_none_20191126... OK
  Applying sentry.0024_auto_20191230_2052...Nothing to do, skipping migration.

 OK
  Applying sentry.0025_organizationaccessrequest_requester... OK
  Applying sentry.0026_delete_event... OK
  Applying sentry.0027_exporteddata... OK
  Applying sentry.0028_user_reports... OK
  Applying sentry.0029_discover_query_upgrade... OK
  Applying sentry.0030_auto_20200201_0039... OK
  Applying sentry.0031_delete_alert_rules_and_incidents... OK
  Applying sentry.0032_delete_alert_email... OK
  Applying sentry.0033_auto_20200210_2137... OK
  Applying sentry.0034_auto_20200210_2311... OK
  Applying sentry.0035_auto_20200127_1711... OK
  Applying sentry.0036_auto_20200213_0106... OK
  Applying sentry.0037_auto_20200213_0140... OK
  Applying sentry.0038_auto_20200213_1904... OK
  Applying sentry.0039_delete_incidentsuspectcommit... OK
  Applying sentry.0040_remove_incidentsuspectcommittable... OK
  Applying sentry.0041_incidenttrigger_date_modified... OK
  Applying sentry.0042_auto_20200214_1607... OK
  Applying sentry.0043_auto_20200218_1903... OK
  Applying sentry.0044_auto_20200219_0018... OK
  Applying sentry.0045_remove_incidentactivity_event_stats_snapshot... OK
  Applying sentry.0046_auto_20200221_1735... OK
  Applying sentry.0047_auto_20200224_2319... OK
  Applying sentry.0048_auto_20200302_1825... OK
  Applying sentry.0049_auto_20200304_0254... OK
  Applying sentry.0050_auto_20200306_2346... OK
  Applying sentry.0051_fix_auditlog_pickled_data... OK
  Applying sentry.0052_organizationonboardingtask_completion_seen... OK
  Applying sentry.0053_migrate_alert_task_onboarding... OK
  Applying sentry.0054_create_key_transaction... OK
  Applying sentry.0055_query_subscription_status... OK
  Applying sentry.0056_remove_old_functions... OK
  Applying sentry.0057_remove_unused_project_flag... OK
  Applying sentry.0058_project_issue_alerts_targeting... OK
  Applying sentry.0059_add_new_sentry_app_features... OK
  Applying sentry.0060_add_file_eventattachment_index... OK
  Applying sentry.0061_alertrule_partial_index... OK
  Applying sentry.0062_key_transactions_unique_with_owner... OK
  Applying sentry.0063_drop_alertrule_constraint... OK
  Applying sentry.0064_project_has_transactions... OK
  Applying sentry.0065_add_incident_status_method... OK
  Applying sentry.0066_alertrule_manager... OK
  Applying sentry.0067_migrate_rules_alert_targeting... OK
  Applying sentry.0068_project_default_flags... OK
  Applying sentry.0069_remove_tracked_superusers... OK
  Applying sentry.0070_incident_snapshot_support... OK
  Applying sentry.0071_add_default_fields_model_subclass... OK
  Applying sentry.0072_alert_rules_query_changes... OK
  Applying sentry.0073_migrate_alert_query_model... OK
  Applying sentry.0074_add_metric_alert_feature... OK
  Applying sentry.0075_metric_alerts_fix_releases... OK
  Applying sentry.0076_alert_rules_disable_constraints... OK
  Applying sentry.0077_alert_query_col_drop_state... OK
  Applying sentry.0078_incident_field_updates... OK
  Applying sentry.0079_incidents_remove_query_field_state... OK
  Applying sentry.0080_alert_rules_drop_unused_tables_cols... OK
  Applying sentry.0081_add_integraiton_upgrade_audit_log... OK
  Applying sentry.0082_alert_rules_threshold_float... OK
  Applying sentry.0083_add_max_length_webhook_url... OK
  Applying sentry.0084_exported_data_blobs... OK
  Applying sentry.0085_fix_error_rate_snuba_query... OK
  Applying sentry.0086_sentry_app_installation_for_provider... OK
  Applying sentry.0087_fix_time_series_data_type... OK
  Applying sentry.0088_rule_level_resolve_threshold_type... OK
  Applying sentry.0089_rule_level_fields_backfill... OK
  Applying sentry.0090_fix_auditlog_pickled_data_take_2... OK
  Applying sentry.0091_alertruleactivity... OK
  Applying sentry.0092_remove_trigger_threshold_type_nullable... OK
  Applying sentry.0093_make_identity_user_id_textfield... OK
  Applying sentry.0094_cleanup_unreferenced_event_files... OK
  Applying sentry.0095_ruleactivity... OK
  Applying sentry.0096_sentry_app_component_skip_load_on_open... OK
  Applying sentry.0097_add_sentry_app_id_to_sentry_alertruletriggeraction... OK
  Applying sentry.0098_add-performance-onboarding... OK
  Applying sentry.0099_fix_project_platforms... OK
  Applying sentry.0100_file_type_on_event_attachment... OK
  Applying sentry.0101_backfill_file_type_on_event_attachment... OK
  Applying sentry.0102_collect_relay_analytics... OK
  Applying sentry.0103_project_has_alert_filters... OK
  Applying sentry.0104_collect_relay_public_key_usage... OK
  Applying sentry.0105_remove_nullability_of_event_attachment_type... OK
  Applying sentry.0106_service_hook_project_id_nullable... OK
  Applying sentry.0107_remove_spaces_from_slugs... OK
  Applying sentry.0108_update_fileblob_action... OK
  Applying sentry.0109_sentry_app_creator... OK
  Applying sentry.0110_sentry_app_creator_backill... OK
  Applying sentry.0111_snuba_query_event_type... OK
  Applying sentry.0112_groupinboxmodel... OK
  Applying sentry.0113_add_repositoryprojectpathconfig... OK
  Applying sentry.0114_add_unhandled_savedsearch... OK
  Applying sessions.0001_initial... OK
  Applying sites.0001_initial... OK
  Applying sites.0002_alter_domain_unique... OK
  Applying social_auth.0001_initial... OK
12:45:43 [WARNING] sentry: Cannot initiate onboarding for organization (1) due to missing owners
Created internal Sentry project (slug=internal, id=1)

Would you like to create a user account now? [Y/n]: y
Email: hidden@example.org
Password: 
Repeat for confirmation: 
User created: hidden@example.org
Added to organization: sentry
Creating missing DSNs
Correcting Group.num_comments counter
Unable to find image 'alpine:latest' locally
latest: Pulling from library/alpine
188c0c94c7c5: Already exists
Digest: sha256:c0e9560cda118f9ec63ddefb4a173a2b2a0347082d7dff7dc14272e7841a5b5a
Status: Downloaded newer image for alpine:latest

Generating Relay credentials...
Relay credentials written to relay/credentials.json

----------------
You're all done! Run the following command to get Sentry running:

  docker-compose up -d
```
</p>
</details>

Up docker-compose environment
```shell
docker-compose up -d
```
<details><summary>output</summary>
<p>

```log
Starting sentry_onpremise_redis_1                ... done
Starting sentry_onpremise_zookeeper_1            ... done
Creating sentry_onpremise_symbolicator-cleanup_1 ... done
Starting sentry_onpremise_symbolicator_1         ... done
Starting sentry_onpremise_memcached_1            ... done
Starting sentry_onpremise_smtp_1                 ... done
Starting sentry_onpremise_postgres_1             ... done
Starting sentry_onpremise_clickhouse_1           ... done
Starting sentry_onpremise_kafka_1                ... done
Starting sentry_onpremise_snuba-sessions-consumer_1     ... done
Starting sentry_onpremise_snuba-replacer_1              ... done
Creating sentry_onpremise_relay_1                       ... done
Starting sentry_onpremise_snuba-outcomes-consumer_1     ... done
Starting sentry_onpremise_snuba-consumer_1              ... done
Starting sentry_onpremise_snuba-transactions-consumer_1 ... done
Starting sentry_onpremise_snuba-api_1                   ... done
Creating sentry_onpremise_snuba-cleanup_1               ... done
Creating sentry_onpremise_ingest-consumer_1             ... done
Creating sentry_onpremise_web_1                         ... done
Creating sentry_onpremise_sentry-cleanup_1              ... done
Creating sentry_onpremise_post-process-forwarder_1      ... done
Creating sentry_onpremise_worker_1                      ... done
Creating sentry_onpremise_cron_1                        ... done
Creating sentry_onpremise_nginx_1                       ... done
```
</p>
</details>

Check result
```shell
docker-compose ps
```
<details><summary>output</summary>
<p>

```log
                     Name                                   Command               State              Ports            
----------------------------------------------------------------------------------------------------------------------
sentry_onpremise_clickhouse_1                    /entrypoint.sh                   Up      8123/tcp, 9000/tcp, 9009/tcp
sentry_onpremise_cron_1                          /bin/sh -c exec /docker-en ...   Up      9000/tcp                    
sentry_onpremise_ingest-consumer_1               /bin/sh -c exec /docker-en ...   Up      9000/tcp                    
sentry_onpremise_kafka_1                         /etc/confluent/docker/run        Up      9092/tcp                    
sentry_onpremise_memcached_1                     docker-entrypoint.sh memcached   Up      11211/tcp                   
sentry_onpremise_nginx_1                         nginx -g daemon off;             Up      0.0.0.0:9000->80/tcp        
sentry_onpremise_post-process-forwarder_1        /bin/sh -c exec /docker-en ...   Up      9000/tcp                    
sentry_onpremise_postgres_1                      docker-entrypoint.sh postgres    Up      5432/tcp                    
sentry_onpremise_redis_1                         docker-entrypoint.sh redis ...   Up      6379/tcp                    
sentry_onpremise_relay_1                         /bin/bash /docker-entrypoi ...   Up      3000/tcp                    
sentry_onpremise_sentry-cleanup_1                /entrypoint.sh 0 0 * * * g ...   Up      9000/tcp                    
sentry_onpremise_smtp_1                          docker-entrypoint.sh exim  ...   Up      25/tcp                      
sentry_onpremise_snuba-api_1                     ./docker_entrypoint.sh api       Up      1218/tcp                    
sentry_onpremise_snuba-cleanup_1                 /entrypoint.sh */5 * * * * ...   Up      1218/tcp                    
sentry_onpremise_snuba-consumer_1                ./docker_entrypoint.sh con ...   Up      1218/tcp                    
sentry_onpremise_snuba-outcomes-consumer_1       ./docker_entrypoint.sh con ...   Up      1218/tcp                    
sentry_onpremise_snuba-replacer_1                ./docker_entrypoint.sh rep ...   Up      1218/tcp                    
sentry_onpremise_snuba-sessions-consumer_1       ./docker_entrypoint.sh con ...   Up      1218/tcp                    
sentry_onpremise_snuba-transactions-consumer_1   ./docker_entrypoint.sh con ...   Up      1218/tcp                    
sentry_onpremise_symbolicator-cleanup_1          /entrypoint.sh 55 23 * * * ...   Up      3021/tcp                    
sentry_onpremise_symbolicator_1                  /bin/bash /docker-entrypoi ...   Up      3021/tcp                    
sentry_onpremise_web_1                           /bin/sh -c exec /docker-en ...   Up      9000/tcp                    
sentry_onpremise_worker_1                        /bin/sh -c exec /docker-en ...   Up      9000/tcp                    
sentry_onpremise_zookeeper_1                     /etc/confluent/docker/run        Up      2181/tcp, 2888/tcp, 3888/tcp
```
</p>
</details>

## Login

After docker-compose environment is up and running, it is possible to login sentry via http://127.0.0.1:19000
