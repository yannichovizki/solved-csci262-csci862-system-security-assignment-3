Download Link: https://assignmentchef.com/product/solved-csci262-csci862-system-security-assignment-3
<br>
<h1>Overview</h1>

Each group needs to design and implement a honeypot event modeller &amp; IDS/auditor system in accordance with the descriptions below. It’s a honeypot in the sense that you are to be able to produce events consistent with provided statistics, so that if those statistics accurately represented the real world your system and the real world would be indistinguishable to a certain level of detail. While there are concrete details on the form of the initial input, and certain inputs along the way, the format of intermediate data is up to each group.

The implementation is to be in C, C++ or Java. Your program must compile on Banshee with instructions you provide in a file Readme.txt. Sample input instructions will be specified for a program compiled to Traffic.

You need to provide a report in a file Report.pdf covering the various points through this assignment where information is required. This report should be broken into sections associated with the components:

<ul>

 <li>Initial input.</li>

 <li>Activity engine and the logs.</li>

 <li>Analysis engine.</li>

 <li>Alert engine.</li>

</ul>

<h1>Initial Input</h1>

You only need command line options at the setup phase, some user input is required later.

Traffic Vehicles.txt Stats.txt Days java Traffic Vehicles.txt Stats.txt Days

Days is an integer used in the next section. All activity is associated with a single road.

Here goes an example Vehicles.txt file. This file describes the vehicles and some of their parameters.

6 Bus:0:LLLDDD:3:2:

Motorbike:0:LLDDLL:1:1:

Red car:1:LDLLDL:1:1:

Elephant:1:LD:2:5:

Taxi:0:LDDL:0:2: Emergency:0:LLDD:3:0:

The first line contains the number of vehicle types being monitored, those are the only vehicles that travel on the road. Each subsequent line is of the form

Vehicle name:Parking flag:Registration format:Volume weight:Speed weight:

The Parking flag indicates whether the vehicle type is allowed to park at the side of the road, with a 0 used to indicate they cannot and a 1 used to indicate they can. The Registration format uses L for letter and D for digit to indicate the format of the registration plate for the vehicle. For example, for the data above, a Bus might have a plate ABC012, a Taxi X44T, and an Elephant M1. The Vehicle name is simply that, a name, and you should not attempt to modify the behaviour based on the name.

The Volume weight and Speed weight are used in the alert engine and will always be non–negative integers. They indicate the significance of anomalous behaviour by a particular Vehicle type. For example, Emergency vehicles are allowed to speed, so the speed weight is 0; it’s not a problem; while Elephants speeding should be seen as a significant problem, particular since the speed set in the example, see below, is 60 km/h and African Elephants can only reach about 40 km/h.

The file Stats.txt contains the distributions to be modelled for the events. This is distinct from the above to facilitate an extension to multiple roads. Here goes an example Stats.txt file.

6 5 60 20

Bus:3:1:40:10:

Motorbike:10:3:55:5:

Red car:20:4:50:4:

Elephant:2:1:10:10:

Taxi:5:2:53:7:

Emergency:1:0.5:60:10:

The four numbers on the first line are all positive integers. They represent, respectively:

<ul>

 <li>The number of vehicle types being monitored.</li>

 <li>The length of the road in kilometres.</li>

 <li>The speed limit in kilometres per hour. This is the same across the whole road.</li>

 <li>The number of parking spaces available.</li>

</ul>

Each subsequent line is of the form

Vehicle type:Number mean:Number standard deviation:Speed mean: Speed standard deviation:

The means and standard deviations do not need to be integers but will be specified to at most 2 decimal places.

Your program should appropriately report vehicle types and statistics read in, as evidence this phase works. If you pick up problems with the reading in these files, so they don’t exist or an incorrectly formatted, you should report the problem and abort.

You should include in your report a description of:

<ol>

 <li>How you are going to store the vehicle types and statistics internally.</li>

 <li>Potential inconsistencies within and between txt and Stats.txt. You should attempt to detect (at least some of) those inconsistencies. If there are inconsistencies you are aware of but haven’t attempted to detect them, note this in your report.</li>

</ol>

<h1>Activity Engine and the Logs</h1>

Once the intial setup has taken place, and you have read in the base files, the activity engine should start generating and logging events.

Time should be stepped in 1-minute blocks. Your program should give some indication as to what is happening, without being verbose. So, you might indicate that you have started this phase and as it is processing indicate the currently day being generated.

There are five types of events. The first is associated with vehicle generation, the other four with existing vehicle activity.

<ol>

 <li>Vehicle arrival, always at the start of the road. This is only up to 23:00 on any given day.</li>

 <li>Vehicle departure via a side road. The vehicle is then out of the road system.</li>

 <li>Vehicle departure via the end of the road. The vehicle is then out of the road system.</li>

 <li>Vehicle parks or stops parking.</li>

 <li>Vehicle moves and possibly changes speed.</li>

</ol>

You should think about appropriate probabilities for each of those activities. The statistics associated with vehicle volume are tied to the vehicle arrival. The statistics associated with vehicle speed are tied to the speed of the vehicle when it arrives on the road. You only test breaches of speed limit using the average speed across the whole road based on the times, so you don’t need to record the speed of the vehicle at each time step.

You are attempting to produce statistics approximately consistent with the statistics specified in the statistics file. You should log for the number of Days specified at the initial running of Traffic. You can, if you like, store the activities in distinct files for each day, or in a single log file. This collection of events forms the baseline data for the system.

You can clear all activity at midnight. Every vehicle left on the road, if there are any, simply disappears <em>^</em>¨ .

You should include in your report a description of:

<ol>

 <li>The process used to generate events approximately consistent with the particular distribution. While the vehicle arrivals are discrete events, the speed of the vehicle is effectively continuous so the way you generate something in accordance with the distribution will likely differ.</li>

 <li>The name and format of the log file, with justification for the format. You will need to be able toread the log entries for subsequent parts of the program. The log file needs to be human readable.</li>

 <li>Any alarms that may be raised during the activity, so an immediate detection of a problem.</li>

</ol>

<h1>Analysis Engine</h1>

Your program should indicate it has completed event generation and is going to begin analyis. You can now measure the baseline data for the traffic and determine the statistics associated with the baseline.

Produce totals for each vehicle type for each day, store that in a data file, and determine the mean and standard deviation associated with that vehicle volume and speed across that data. Report what is happening as you consider appropriate. Note again the speed statistics are based just on the initial arrival speed of the vehicle.

Your analysis engine should also test for average speed breaches for each vehicle that goes off the end of the road, not for those that depart by side streets. You calculate the average speed based of it’s arrival and departure times and you don’t need to take into account any parking the vehicle may do. For each day, you can produce a list of vehicles that have breached the average speed limit.

Even if you are unable to produce data consistent with a given distribution you can still have the analysis engine reading and reporting on the log file.

You should include in your report a description of:

<ol>

 <li>Specify the file containing the daily totals for the events.</li>

 <li>Possible anomalies in reading the logs.</li>

 <li>Possible anomalies in determining the statistics.</li>

</ol>

<h1>Alert Engine</h1>

The alert engine is used to check consistency between” live data” and the base line statistics.

Once this phase is reached you should prompt the user for a file, containing new statistics, and a number of days. The statistics file has the same format as Stats.txt from earlier but will generally be different. You should run your activity engine and produce data for the number of days specified. Use the analysis engine to produce daily totals, those are used in alert detection.

For each day generated you need to report on whether there is are intrusions detected by comparing anomaly counters with thresholds. You calculate anomaly counters by adding up the weighted number of standard deviations each specific tested event value is from the mean for that event, where the standard deviation and mean are those you have generated from the base data and reported, and the weights are taken from the original Vehicles.txt file. There are to be distinct analyses and reports for the Vehicle volume and for the Vehicle speed, so there is an anomaly counter for each. We will go through an example of this in class.

For example, if the mean number of Elephants per day is 2 and the standard deviation is 1; then if we get 1 Elephant in a day we are 1 standard deviation from the mean. Referrring back to the weight of the Elephant vehicle type we see it was 2 so the Elephant contributes 2 to our overall anomaly counter. There would be a similar analysis for Elephant speed.

The thresholds for detecting an intrusion are 2∗(Sums of weights) where the weights are taken from Vehicles.txt, and they are across speeds or volumes depending on which threshold you are looking at. If an anomaly counter is greater than associated threshold you should report this as an anomaly.

You should output the thresholds at the start, since they are fixed, and for each day give the values of the two anomaly counters as well as reporting a Volume anomaly and/or a Speed anomaly or No anomaly.

Once the alert engine part has finished you should return to the start of this phase, so another set of statistics and number of days can be considered. An option to quit should be provided.

<h1>Some final notes</h1>

In addition to addressing the specified points, you can include anything of significance in your report. You should identify any parts not completed.

The data files will be generated on Banshee and used on Banshee. When you are transferring files from Moodle to Banshee for use, be careful that control characters aren’t added at the end of lines and so on. The utility dos2unix may help sort out the format if you are unsure; and if you have no idea what this means, please ask!