# Streaming Logs using rsyslog or syslog-ng to Logstash and Smart Cloud Analytics Log Analysis (SCALA)

# Description

This cookbook shows how to use rsyslog and syslog-ng with logstash to stream logs into SCALA using either a custom DSV content pack or the out of the box Syslog Insight Pack. Sample rsyslog and syslog-ng configuration files are provided showing how to configure each to send system logs and application logs in the expected syslog message format to logstash for relay into SCALA.

# What's Included?

1. A sample logstash configuration **(syslog-logstash-scala.conf)**
2. A logstash grok pattern file for rsyslog and syslog-ng messages **(SYSLOGSCALA)**
3. A custom DSV Content Pack properties file **(syslog.props)**
4. An example rsyslog.conf file **(rsyslog.conf)**
5. An example syslog-ng.conf file **(syslog-ng.conf)**

# Deploying the Content Pack

1. Review the sample logstash configuration **syslog-logstash-scala.conf** and determine how you will use the inputs, filters and outputs.  You can use this example or just copy the needed parts into your main logstash configuration file. Pay close attention to the NOTES within the **syslog-logstash-scala.conf** and **SYSLOGSCALA** files.

2. Update the TCP inputs as needed, especially the port numbers you will use. The example logstash configuration file shows four options to explore, in reality you'll likely only use one.

3. Review the filters section and determine if any changes are needed based upon how you use logstash, which version you use, etc. 

4. Place the logstash GROK pattern file **SYSLOGSCALA** into the appropriate location.  If you are using logstash 1.4.2, you may place this into the /patterns directory and you'll be all set. If you are using logstash 1.5 beta, you can place it into the same /patterns directory, but you will need to use the patterns_dir setting to this location in each filter using the GROK patterns (eg multiline, grok).

5. Review the output section and update based on your current logstash integration with SCALA. This example configuration will only work with the upcoming release of the SCALA-Logstash Integration.  If you do not have this, contact Doug McClure for assistance.

6. Copy the DSV Pack properties file **syslog.props** into the $SCALAHOME/unity_content/DSVToolkit_v1.1.0.2 directory.

7. Edit the **syslog.props** file and update the scalaHome directory path.  You may also want to optimize the various index configuration fields in this file. For example, setting filterable = false for the processID field if you don't feel faceting on processID would be useful.

8. From the $SCALAHOME/unity_content/DSVToolkit_v1.1.0.2 directory, execute this command to build and deploy the DSV Content Pack into SCALA: **python dsvGen.py syslog.props -d -f -u unityadmin -p unityadmin**

9. Create a SCALA datasource which uses the new DSV Content Pack source type named **syslogDSV** for your rsyslog and syslog-ng flows. Be sure to set the host and path values to match what you're setting in your logstash configuration. By default, they are **host = Syslog-DSVPack and path = Syslog-DSVPack**.

10. Create a SCALA datasource which uses the default SCALA Syslog Insight Pack source type named **Syslog** for your rsyslog and syslog-ng flows. Be sure to set the host and path values to match what you're setting in your logstash configuration. By default, they are **host = Syslog-InsightPack and path = Syslog-InsightPack**.

11. Start up logstash and verify connections from rsyslog or syslog-ng are established. Watch for activity in logstash stdout or within SCALA search results.

# Need Help?

This is a community contributed content pack and no explicit support, guarantee or warranties are provided by IBM nor the contributor. Feel free to engage the community on the ITOAdev forum if you need help!