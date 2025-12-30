# Mapreduce with MRJob

## 1. Exemplary Parameters

1.1. [ssh_address] - example: "pac-ssh.azurehdinsight.net"
    Grupy zasobów  -> "name of the group" (ex bigdata) -> enter to the "HDInsight cluster" -> Properties -> Copy Secure Shell (SSH).
	
1.2. [surname] - example: "buczek"

1.3. [local_repo_path] - example: "C:/Users/vdi-student/hadoop/example_2/" -> local git repository

1.4. [python_path] - example: "C:/Users/vdi-terminal/AppData/Local/Programs/Python/Python312"

## 2. Run locally

2.1. Install Python 3.12
	https://www.python.org/downloads/release/python-3120/

2.2. Virtual environment
```console
    $ [python_path]/python.exe -m venv .venv 
	$ source .venv/Scripts/activate  
	$ python –version
```
	
2.1. Install mrjob / setuptools
```console
    $ pip install mrjob
	$ pip install setuptools 
```

2.2. Run script

2.2.1. Without logs
```console
    $ cd .\hadoop\example_2\
	$ python ratingcount.py data.txt
```

2.2.1. With logs
```console
    $ cd .\hadoop\example_2\
	$ python ratingcount.py data.txt > output.txt --cleanup=NONE
```

## 3. Run on HADOOP cluster

3.1. From local shell copy example_2 to cluster:
```console
	$ ssh sshuser@[ssh_address] "mkdir -p /home/sshuser/[surname]/example_2"
	$ scp -r  [local_repo_path]* sshuser@[ssh_address]:/home/sshuser/[surname]/example_2
```

3.2. Login to the cluster:
```console
    $ ssh sshuser@[ssh_address]
```

3.3. Install mrjob:
```console
    $ sudo pip3 install mrjob
```

3.4. From cluster shell copy data.txt to hdfs:
```console
    $ hdfs dfs -copyFromLocal /home/sshuser/[surname]/example_2/data.txt /user/data-[surname].txt
```
3.5. Run script on cluster:
```console
    $ python3 ./[surname]/example_2/ratingcount.py wasb:///user/data-[surname].txt -r hadoop --hadoop-streaming-jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar --hadoop-tmp-dir wasb:////tmp
```

