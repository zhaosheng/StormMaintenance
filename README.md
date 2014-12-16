# Storm Topology Maintenance

## Check the topology status

Storm topology running status can be found via [Storm UI](http://ec2-54-191-165-197.us-west-2.compute.amazonaws.com:8080/index.html). Our topology is named as **Demo M1**.
If the topology is not showing up or the status is not showing **ACTIVE** there, then please consider killing / restarting the topology.

## Kill the running topology

Click the running topology **Demo M1** and you can see a Kill button under the Topology actions section.

Click the kill and click the OK in the popup. The topology status should be *KILLED*.

Sometimes the topology is showing *ACTIVE* in the UI but the thread cannot be found on the Storm server. Please kill the topology through UI and restart it.

## Check the topology thread

1. SSH to the strom cluster:

		ssh ec2-54-191-165-197.us-west-2.compute.amazonaws.com

1. sudo jps to show all the Java threads:

		[ubuntu@Storm ~]$ sudo jps
		**27843 Topology**
		22377 NameNode
		1447 core
		22784 SecondaryNameNode
		3499 nimbus
		3514 supervisor
		28041 Jps
		22569 DataNode
		20678 worker

	The "Topology" thread should showing up if it is still running.

1. Check the strom log.

		tail -100f `ls -lrt $STORM_HOME/logs/worker-670* | tail -1 | awk '{print $9}'`
			
	You should see below log pattern if everything is going well when refreshing the tracking websites.
	
		2014-12-16 04:38:14 s.p.TagCollector [INFO] Getting tags for URL http://empohs.com/
	
		2014-12-16 04:38:14 s.p.TagCollector [INFO] Loaded (1485) characters from API Call.
	
## Start the Topology

	$STORM_HOME/bin/storm jar /tmp/streaming-1.0-SNAPSHOT-jar-with-dependencies-sh.jar storm.driver.Topology &
	
The console should print **15034 [main] INFO  backtype.storm.StormSubmitter - Finished submitting topology: Demo M1** at the end if the tolopogy is submitted successfully.


