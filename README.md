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

To download and install Sia for your platform, visit [sia.tech](http://sia.tech).

To purchase inexpensive rental contracts with Sia, you have to possess some Siacoin in your wallet. To obtain Siacoin, you will need to purchase some on an exchange such as Bittrex using bitcoin. To obtain bitcoin, you'll need to use a service such as Coinbase to buy bitcoin using a bank account or credit card. If you need help, there are many friendly people active on [Sia's Slack](http://slackin.sia.tech).

#### Configuration
Once you have the Sia Daemon running and synchronized, with rental allowances created, you just need to configure the Minio server to use Sia. Configuration is accomplished using environment variables, and is only necessary if the default values need to be changed. On a linux machine using bash shell, you can easily set environment variables by adding export statements to the "~/.bash_profile" file. For example:
```
export MY_ENV_VAR=VALUE
```
Just remember to reload the profile by executing: "source ~/.bash_profile" on the command prompt.

##### Supported Environment Variables
Environment Variable | Description | Default Value
--- | --- | ---
`SIA_MANAGER_DELAY_SEC` | The number of seconds to delay between cache/db management operations. | 30
`SIA_UPLOAD_CHECK_FREQ_MS` | The number of milliseconds to wait between checks with Sia network to determine if file has completed uploading. | 3000
`SIA_CACHE_MAX_SIZE_BYTES` | The maximum allowed size of the cache directory in bytes. | 10000000000 (10GB)
`SIA_CACHE_PURGE_AFTER_SEC` | The maximum number of seconds since the time a file was last downloaded before removing that file from the cache. | 86400 (24 hours)
`SIA_DAEMON_ADDR` | The address and port of your Sia Daemon instance. | 127.0.0.1:9980
`SIA_CACHE_DIR` | The name of the Sia cache directory. | .sia_cache
`SIA_DB_FILE` | The name of the Sia cache database file. | .sia.db
`SIA_DEBUG` | A flag for enabling debug messages to be printed to the screen. Useful for developers only. 0=Off, 1=On | 0

Most of the default options should work, but they are made available to meet a wide variety of potential needs. If the default values will work for your configuration, no environment variables will need to be set.

#### Running Minio with Sia Gateway
To launch Minio server with the Sia gateway, simply execute the following at the command prompt:
```
./minio gateway sia
```
It should print to the screen a list of access information. To connect to the server and upload files using your web browser, open a web browser and point it to the address displayed for "Browser Access." Then log in using the "AccessKey" and "SecretKey" that are also displayed on-screen. You should then be able to create buckets (folders) and upload files.

You can also interact with the server using Minio's "mc" command line tool. Instructions for using it are also displayed after starting the server.

#### About the Sia Cache
Each time a file is uploaded or downloaded using Minio, the files are passing through a cache layer that sits between the Minio interface and the Sia network. The cache provides many benefits such as drastically lowering the latency experienced with frequently requested files, reducing download fees on the Sia network, etc. Below is a list of things you should know about how the cache operates.
1. When you upload a file through Minio, once Minio confirms the file is uploaded, the file is guaranteed to be "Available" on the Sia network. It may not yet have multiple redundancy, but it is available to be downloaded from Sia if needed. A copy of the uploaded file will exist in the cache directory until either SIA_CACHE_PURGE_AFTER_SEC seconds have elapsed since that file was last downloaded, or until the cache directory runs out of space, as specified by SIA_CACHE_MAX_SIZE_BYTES.
2. When you request a file for download from Minio, if the file exists in the cache, it is served immediately from the cache. If the file does not exist in the cache, it will first be downloaded from the Sia network to the cache directory and then served from the cache.

### Questions?
If you need help, there are many friendly people active on [Sia's Slack](http://slackin.sia.tech).
