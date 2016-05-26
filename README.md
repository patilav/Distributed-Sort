# Distributed Sort

## Introduction 

This readme encompasses the prerequisites, instructions to run the code, 
design choice, performance, results and effort. The goal of the project 
is to mirror shuffle phase of Mapreduce operation using distributed sort 
for climate data set.

## Prerequisites

* Update the script

* We are using Apache Maven 3.3.9 for dependency management and build automation

* Please ```chmod +x buildAndCopyToAWS.sh``` and ```chmod +x masterSlaveScripts.sh```

* AWS CLI

* AWS Instance profile #link http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2_instance-profiles.html 
  the above instance profile should have full access to s3. the reason we use 
  this is to not pass credentials around while also granting full access to s3

* Update /Setup/bootstrap.txt with your details.

* Update /netty-server/params with your details.

## Instructions to run Distributed sort

* ```make setup3``` - for 2 node setup

* ```make setup9``` - for 8 node setup


## Design


	  machines.
	  it wait for a slave to connect.
	  it the metadata about the sort data. This is done intelligently 
	  without overloading any slave with too much data.
	  from all slaves, it decides what chunks of data needs to be transmitted 
	  between slaves.
	  place a file by the name _SUCCESS to the output bucket on S3.
	* When it receives the instruction to send the data to other slaves from 
	  the master, it sends the data and wait to receive data.
	  is done and results are written to S3.

## Bells and Whistles

less data over the network. Based on which buckets data the slave has the 
highest, the master assigns the buckets to the slaves to sort.

## Caveats

  requirement of machines.
  will keep waiting for that message from that slave forever.
  could not be done to lack of time. Can we seen when ssh to machine.


## Configuration and Performance 

* 1 master and 8 slaves nodes.
  is around 8~10 mins.
* Top 10 sorted dry\_bulb_temp values in the entire climate dataset.

	­123.0 24243,19960820,656,­117.4 13773,19991102,1355,­113.6 25501,19990704,
	553,­108.4 14836,19990312,255,­99.6 12921,19980302,1156,­88.6 93226,19980111,
	56,­79.0 13911,20061229,755,­78.0
