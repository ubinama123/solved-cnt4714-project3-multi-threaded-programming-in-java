Download Link: https://assignmentchef.com/product/solved-cnt4714-project3-multi-threaded-programming-in-java
<br>
<strong>Objectives:</strong> To practice programming an application with multiple threads of execution and synchronizing their access to necessary shared objects.

<strong>Description:</strong>  In this programming assignment you will simulate the package shipping management system for an automated package shipping operation similar to the one depicted here:

This example package shipping operation has five routing stations (S0 – S4), each of which has an input and output conveyor connecting to conveyor lines (C0 – C4) that go elsewhere in the system.  Resources were limited when the system was built so each conveyor going to the rest of the facility must be shared between two routing stations.  Since each routing station simultaneously needs an input and output connection to function, access to the shared conveyor lines must be strictly regulated.  Flow direction in not important in our simulation.

You have been hired to design a simulator for a new package management system being built with the same design, but possibly fewer/more stations.  You are to implement this simulator in Java and have each routing station function in its own thread.  A routing station moves packages from one of its connected conveyors to the other.  A station’s workload is the number of times that a routing station needs to have exclusive access to the input and output conveyors during the simulation.  Once a routing station is granted access to both conveyors it calls its doWork()method during which it will flow packages down each of its connected conveyors (of course it must verify that it has access and isn’t in conflict with another routing station).  After the packages-in and packages-out methods are run, the workload of the routing station is reduced by 1 and the routing station will release both of the conveyors and signal waiting routing stations that the conveyors are available.  After executing a flow and releasing its conveyors, a routing station should sleep for some random period of time.  A routing station’s thread stops running when its workload reaches 0.  To prevent deadlock, ensure that each routing station acquires locks on the conveyors it needs in increasing numerical order.




<strong>Restrictions: </strong>

<ol>

 <li>Your source files should begin with comments containing the following information:</li>

</ol>

<strong>/* </strong>

<strong>          Name: <em>&lt;your name goes here&gt;</em> </strong>

<strong>          Course: CNT 4714 Fall 2020 </strong>

<strong>         Assignment title: Project 2 – Multi-threaded programming in Java </strong>

<strong>          Date:  October 4, 2020 </strong>

<strong> </strong>

<strong>          Class:  <em>&lt;name of class goes here&gt; </em></strong>

<strong>*/ </strong>

<ol start="2">

 <li><strong>Do not</strong> use a monitor to control the synchronization in your program (i.e., do not use the Java synchronize statement).</li>

</ol>




<strong>Input Specification:</strong>

Your program must initially read from a text file (config.txt) to gather configuration information for the simulator.  The first line of the text file will be the number of routing stations to use during the simulation.  Afterwards, there will be one line for each station.  These lines will hold the amount of work each station needs to process (i.e, the number of times it needs to move packages down the conveyor system).  Only use integers in your configuration file, decimals will not be needed.  You can assume that the maximum number of stations will be 10.

<strong> </strong>

<strong>Output Specification:</strong>

Your simulator must output the following text to let the user know what the simulator is doing in each of these situations:




<ol>

 <li>An input conveyor is set:</li>

</ol>

<strong>Routing Station X: input connection is set to conveyor number n</strong>




<ol start="2">

 <li>An output conveyor is set:</li>

</ol>

<strong>Routing Station X: output connection is set to conveyor number n </strong>




<ol start="3">

 <li>A stations workload is set:</li>

</ol>

<strong>Routing Station X: Workload set. Station X has a total of n package groups to move.</strong>




<ol start="4">

 <li>A station is granted access to its input conveyor:</li>

</ol>

<strong>Station X: holds lock on input conveyor n</strong>







<ol start="5">

 <li>A station is granted access to its output conveyor:</li>

</ol>

<strong>Station X: holds lock on output conveyor n</strong>




<ol start="6">

 <li>A station releasing access to its input conveyor:</li>

</ol>

<strong>Station X: unlocks input conveyor n</strong>




<ol start="7">

 <li>A station releasing access to its output conveyor:</li>

</ol>

<strong>Station X: unable to lock output conveyor – releasing lock on input conveyor n.</strong>




<ol start="8">

 <li>A station cannot obtain its second lock (on the output conveyor):</li>

</ol>

<strong>Station X: unlocks output conveyor n  </strong>

<ol start="9">

 <li>A station successfully flows packages on input conveyor:</li>

</ol>

<strong>Station X: successfully moves packages into station on input conveyor n. </strong>

<strong> </strong>

<ol start="10">

 <li>A station successfully flows packages on output conveyor:</li>

</ol>

<strong>Station X: successfully moves packages out of station on output conveyor n.  </strong>

<ol start="11">

 <li>A station completes a flow:</li>

</ol>

<strong>Station X: has n package groups left to move.  </strong>

<ol start="12">

 <li>A station has completed its workload:</li>

</ol>

<strong>* * Station X: Workload successfully completed. * *</strong>

<strong> </strong>

<strong> </strong>

<strong>Deliverables: </strong>




Submit the following items via WebCourses no later than 11:59pm October 4, 2020.

<ul>

 <li>All of your .java</li>

 <li>A copy of a sample execution of your program, i.e., the output produced by your simulator (this should just be a text file). In your IDE redirect console output to a file, do this and include a copy of the output file produced by your program.</li>

</ul>




<strong>Additional Information: </strong>

Actual simulation run in Eclipse (console output redirected in this example) with <strong>config.txt containing 3 2 3 4</strong>, is shown below.

The example scenario below may help you to understand what is happening with a routing station and the acquisition of locks and work flow. Illustration is based only on S0 from the original configuration.




<strong><u>Illustration of the timeline of a station in execution</u> </strong>

<strong>Time 1:</strong>  Station 0 needs to move a package group from input conveyor (conveyor 0) to the output conveyor (conveyor 4).  It must acquire the locks on conveyor 0 and conveyor 4, in that order.  If Station 1, currently holds the lock on conveyor 0, then Station 0 is blocked and must wait for conveyor 0 to become available.  Note that Station 0 cannot attempt to lock conveyor 4 if it is unable to obtain the lock on conveyor 0.

<strong>Time 2:</strong>  Station 0 acquires lock on conveyor 0.  Attempts to lock conveyor 4.  If conveyor 4 is not available (Station 4 currently owns lock on conveyor 4), then Station 0 must release lock on conveyor 0 and block until both conveyor 0 and conveyor 4 can be obtained.

<strong>Time 3:</strong>  Station 0 has acquired locks on conveyor 0 and conveyor 4.  Packages now moving.

<strong>Time 4:</strong>  Station 0 has successfully moved 1 workload unit of packages from input side to output side.  Station 0 releases locks on conveyor 0 and conveyor 4.  Station 0 goes idle for random period of time if its workload is not yet 0, otherwise, Station 0 terminates.

packages moved