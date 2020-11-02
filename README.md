# Docker Compose for Alluxio

This repository aims to provide a Alluxio Cluster for tests by docker-compose. This Alluxio Cluster is configured to mount a s3 bucket.

## Prerequisites

Must have an aws credentials file configured in the user's path. (e.g: /home/${USER)/.aws/credentials)

## Usage

It's necessary to export the ip of your local host so that external clients to the docker network can access workers nodes.

### Mac OS

```shell script
export EXTERNAL_IP=$(ipconfig getifaddr en0)
```

### Linux

```shell script
export EXTERNAL_IP=$(hostname -I | awk '{print $1}')
```

### Enable Access For Clients

Reference: https://docs.alluxio.io/os/user/stable/en/ufs/S3.html#identity-and-access-control-of-s3-objects

Find and export aws canonical id

```shell script
export AWS_CANONICAL_ID=$(aws s3api list-buckets --query "Owner.ID" | sed "s/\"//g")
```

Export local user

```shell script
export CLIENT_USER=${USER}
```

### Mounting s3 bucket in Alluxio Cluster

```shell script
docker exec alluxio-master bash -c "./bin/alluxio fs mount --readonly alluxio://alluxio-master:19998/{INNER MNT PATH ALLUXIO} s3://{YOUR BUCKET S3} --shared"
```