# Minio with Sia Gateway
This project is a fork of the official Minio project located [here](http://github.com/minio/minio). This project provides a version of Minio that supports using Sia for backend storage.

### What is Minio?
Minio is an object storage server released under Apache License v2.0. It is compatible with Amazon S3 cloud storage service. It is best suited for storing unstructured data such as photos, videos, log files, backups and container / VM images. Size of an object can range from a few KBs to a maximum of 5TB.

### What is Sia?
Sia is a blockchain-based decentralized storage service with built-in privacy and redundancy that costs up to 10x LESS than Amazon S3 and most other cloud providers! See [sia.tech](https://sia.tech) to learn how awesome Sia truly is.

### Getting Started
Since the Sia integration is not yet officially supported by Minio, no official binaries or packages are currently available for download. However, installing from source is a very simple process.

#### Install Go
Building Minio requires that you have the Go programming language installed. [Download and install it from here](https://golang.org/dl/).

#### Clone this Project
You'll want to clone this project into your system's $GOROOT/src/github.com/minio directory. For example:
```
mkdir /Users/david/go/src/github.com/minio
cd /Users/david/go/src/github.com/minio
git clone http://github.com/dvstate/minio
```

#### Build the Minio Server
Then you'll need to build the Minio server executable, which is a simple process thanks to the Makefile provided by Minio. For example:
```
cd /Users/david/go/src/github.com/minio/minio
make
```
After the build process completes, an executable named 'minio' will appear in the same directory. This is the executable you will launch to run the Minio server.

#### Install Sia Daemon
To use Sia for backend storage, Minio will need access to a running Sia daemon that is:
1. fully synchronized with the Sia network,
2. has sufficient rental contract allowances, and
3. has an unlocked wallet.
