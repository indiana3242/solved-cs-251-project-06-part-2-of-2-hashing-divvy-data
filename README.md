Download Link: https://assignmentchef.com/product/solved-cs-251-project-06-part-2-of-2-hashing-divvy-data
<br>
<span style="font-size: 2em;">Overview</span>

In part 1 you built a <strong>hashmap</strong> class for storing (key, value) pairs in a hash table.  Here in part 2 we’re going to use this class to store and analyze data from the <strong>DIVVY</strong> bike sharing company.




Your program is going to input 2 types of data:  stations in the DIVVY system, and bike trips that riders have taken on a DIVVY bike.  Your program needs to lookup stations by their station id (integer) or

their station abbreviation (string).  This implies you’ll need two hashmaps, one to lookup by id and another to lookup by string.  Your program will also need another hashmap to lookup trips, and another hashmap to lookup bikes.  In total, your program will have at least 4 hashmap variables:




int main()

{    hashmap&lt;?, ?&gt;  <strong>stationsById</strong>(?);    hashmap&lt;?, ?&gt;  <strong>stationsByAbbrev</strong>(?);    hashmap&lt;?, ?&gt;  <strong>tripsById</strong>(?);    hashmap&lt;?, ?&gt;  <strong>bikesById</strong>(?);







It’s very important to separate the abstraction — hashmap — from the # of instances that you need to store all the data.  The hashmap class implements a hash table.  This one class is used to create 4 different hash tables to store the data needed by the program.  Note that you’ll also need a separate hash function to work with each hashmap instance:  HashByStationID, HashByStationAbbrev, etc.  You cannot use the built-in hash functions.

<h2>Assignment</h2>

The program inputs data from two files, whose filenames are entered by the user.  The 1<sup>st</sup> file contains stations data, and the 2<sup>nd</sup> file contains trip data.  The program then offers a choice of 7 commands, and repeats until the user enters #.  The screenshot to the right shows the output from the “help” command ————-&gt;  The 6 commands are summarized as follows:




<ol>

 <li>Lookup station by id</li>

 <li>Lookup station by abbreviation (e.g. “Adler”)</li>

 <li>Lookup a bike trip by trip id (e.g. “Tr10426561”)</li>

 <li>Lookup bike usage by bike id (e.g. “B5218”)</li>

 <li>Find nearby stations based on latitude / longitude</li>

 <li>Find similar trips based on latitude / longitude</li>

</ol>




A set of input files are provided for testing purposes:  stations.csv, trips.csv, and trips-2016-q1.csv.  To help you with finding nearby stations based on latitude and longitude, a function <strong>distBetween2Points(lat1, long1, lat2, long2)</strong> is provided in the file “util.cpp”.  The corresponding header file “util.h” is also provided.  You can find these files on the course dropbox under Projects, <a href="https://www.dropbox.com/sh/zrb6udkfu88puyu/AAAPcndf6HxCh8JfPORiffVKa?dl=0">project06</a><a href="https://www.dropbox.com/sh/zrb6udkfu88puyu/AAAPcndf6HxCh8JfPORiffVKa?dl=0">–</a><a href="https://www.dropbox.com/sh/zrb6udkfu88puyu/AAAPcndf6HxCh8JfPORiffVKa?dl=0">part</a><a href="https://www.dropbox.com/sh/zrb6udkfu88puyu/AAAPcndf6HxCh8JfPORiffVKa?dl=0">02</a><a href="https://www.dropbox.com/sh/zrb6udkfu88puyu/AAAPcndf6HxCh8JfPORiffVKa?dl=0">–</a><a href="https://www.dropbox.com/sh/zrb6udkfu88puyu/AAAPcndf6HxCh8JfPORiffVKa?dl=0">files</a><a href="https://www.dropbox.com/sh/zrb6udkfu88puyu/AAAPcndf6HxCh8JfPORiffVKa?dl=0">.</a>  If you are working on Codio, use the File menu, Upload command to move these files to your Codio project.  Note that in order to call the provided function from main.cpp, you’ll need to #include “util.h”.  Then you’ll need to modify your makefile to compile “util.cpp” as part of the program:




build:  rm -f program.exe

g++ -O2 -std=c++11 -Wall main.cpp hash.cpp <strong>util.cpp</strong> -o program.exe

<h2>Input Files</h2>

The input files are text files in <strong>CSV</strong> format — i.e. comma-separated values.  The first file defines the set of <strong>stations</strong>, in no particular order.  Here are the first few lines from the provided “stations.csv” file:




<strong>id,abbrev,fullname,latitude,longitude,capacity,online_date </strong>

456,2112 W Peterson,2112 W Peterson Ave,41.991178,-87.683593,15,5/12/2015

101,63rd Beach,63rd St Beach,41.78101637,-87.57611976,23,4/20/2015 109,900 W Harrison,900 W Harrison St,41.874675,-87.650019,19,8/6/2013 .

.

.




The first line contains column headers, and should be ignored.  The data starts on line 2, and each data line represents one Divvy station for docking/undocking bicycles.  Each station contains 7 values:  a unique integer ID, a unique abbreviated name, the stations’s fullname, latitude, longitude, the capacity of the station (i.e. the # of bikes you can dock there), and the date the station came online in the DIVVY system.  Assume the data is valid and properly formatted, however do not assume the station IDs are contiguous 1..N, they are not.  For hashing purposes, use the station’s ID and abbreviated name, these are guaranteed to uniquely identify each station.




The second file defines a set of <strong>trips</strong>, in no particular order.  With DIVVY, a “bike trip” means a rider checks out a bike from one station, rides it around the city, and then checks it back into the same station, or a different station.  The main purpose of DIVVY is commuting, so most bike rides are short — less than 30 minutes.  Here are the first few lines from the provided “trips.csv” file:




<strong>tripid,starttime,stoptime,bikeid,duration,from,to,identifies,birthyear </strong>Tr10426561,6/30/2016 23:35,7/1/2016 0:02,B5229,1620,329,307,Male,1968

Tr10426434,6/30/2016 23:09,6/30/2016 23:35,B5218,1547,228,253,Female,1988

Tr10426287,6/30/2016 22:48,6/30/2016 23:13,B4199,1521,145,35,, Tr10426168,6/30/2016 22:31,6/30/2016 22:42,B2659,668,113,69,Male,1986 .

.

.




The first line contains column headers, and should be ignored.  The data starts on line 2, and each data line represents one DIVVY trip.  Each data line consists of 9 values:




<table width="485">

 <tbody>

  <tr>

   <td width="24">•</td>

   <td width="120">Trip id:</td>

   <td width="340">string consisting of “Tr” following by 1 or more digits</td>

  </tr>

  <tr>

   <td width="24">•</td>

   <td width="120">Start time:</td>

   <td width="340">date and time of trip start</td>

  </tr>

  <tr>

   <td width="24">•</td>

   <td width="120">Stop time:</td>

   <td width="340">data and time of trip end</td>

  </tr>

  <tr>

   <td width="24">•</td>

   <td width="120">Bike id:</td>

   <td width="340">string consisting of “B” followed by 1 or more digits</td>

  </tr>

  <tr>

   <td width="24">•</td>

   <td width="120">Trip duration:</td>

   <td width="340">integer, in seconds</td>

  </tr>

  <tr>

   <td width="24">•</td>

   <td width="120">From station id:</td>

   <td width="340">integer, station where bike was checked out</td>

  </tr>

  <tr>

   <td width="24">•</td>

   <td width="120">To station id:</td>

   <td width="340">integer, station where bike was returned</td>

  </tr>

  <tr>

   <td width="24">•</td>

   <td width="120">Identifies:</td>

   <td width="340">gender the rider identifies as (string, optional)</td>

  </tr>

  <tr>

   <td width="24">•</td>

   <td width="120">Birth year:</td>

   <td width="340">the rider’s year of birth (integer, optional)</td>

  </tr>

 </tbody>

</table>




Assume the data is valid and properly formatted.  This implies you may assume the station ids — the <strong>from</strong> and <strong>to</strong> station ids — exist in the provided stations file.  In case you’re curious, the DIVVY data is available for browsing on Chicago’s data <a href="https://data.cityofchicago.org/Transportation/Divvy-Trips-Dashboard/u94x-unre">portal</a><a href="https://data.cityofchicago.org/Transportation/Divvy-Trips-Dashboard/u94x-unre">;</a> the raw data is from DIVVY @ <a href="https://www.divvybikes.com/data">https://www.divvybikes.com/data</a><a href="https://www.divvybikes.com/data">.</a>




With hashing, it’s important to know how much data to expect, so that appropriate-sized hash tables can be allocated.  We’re going to assume a load factor of 5, meaning the hash tables will be 5x larger than necessary to reduce collisions (i.e. 20% full, 80% empty).  Expect at most 2,000 stations, 10,000 bikes, and 500,000 trips.  This implies your hashmaps should have the following sizes:




int main()

{

hashmap&lt;?, ?&gt;  <strong>stationsById</strong>(10000);      // 10K    hashmap&lt;?, ?&gt;  <strong>stationsByAbbrev</strong>(10000);  // 10K    hashmap&lt;?, ?&gt;  <strong>tripsById</strong>(2500000);       // 2.5M    hashmap&lt;?, ?&gt;  <strong>bikesById</strong>(50000);         // 50K




<table width="416">

 <tbody>

  <tr>

   <td width="6"></td>

   <td width="410"><strong>Command #1:  lookup station by id </strong></td>

  </tr>

 </tbody>

</table>

<table width="416">

 <tbody>

  <tr>

   <td width="6"></td>

   <td width="410"><strong>Command #2:  lookup station by abbreviation </strong></td>

  </tr>

 </tbody>

</table>

If the user inputs a numeric string, assume it’s a station id and search for the station by this id.  If found, output the station information, otherwise output “Station not found”.

If the user inputs a string that doesn’t match any of the other commands, assume it’s an abbreviated station name and search for the station by this abbreviation.  If found, output the station information, otherwise output “Station not found”.




An interesting design decision is how do you store the data.  Since you’ll have 2 hashmaps, one by id and one by abbreviation, do you store the station data twice?  That seems wasteful.  What’s are some alternative?  What’s the best approach that saves memory yet doesn’t overly complicate the programming?

<h2>Command #3:  lookup trip by tripid</h2>

If the user inputs a string starting with “Tr” and followed by 1 or more digits, assume they have entered a tripid.  Search for the trip by this id, and output the trip information if found.  If not, output “Trip not found”.




Notice the duration is converted to hours, minutes and seconds.  Only output the hours if the hours are &gt; 0; likewise, only output the minutes if the minutes are &gt; 0.  If the case of 1 hour, or 1 minute, or 1 second, the program outputs (incorrectly) 1 hours, or 1 minutes, or 1 seconds.  You don’t have to worry about the if statement to correct for this special case.




Also notice that not all riders choose to supply their birthyear, or their identifying gender.  In this case output nothing.










<h2>Command #4:  lookup bike usage by bikeid</h2>

If the user inputs a string starting with “B” and followed by 1 or more digits, assume they have entered a bikeid.  Search for a bike by this id, and output the # of trips that involved this bike.  If the user enters a bikeid that doesn’t exist (i.e. was never used in any trip), output “Bike not found”.




You could compute this on demand by searching through all the trips, but this linear search is expensive on real trip files (that contain 100,000+ trips).  Instead, as you input the trip data, build a hashmap of each bike and accumulate the # of trips in which it was used.  That way you can answer the query in O(1) time instead of O(N) time.

<h2>Command #5:  find nearby stations based on lat / long</h2>

You are no doubt familiar with finding things that are nearby.  This is easily done with a phone based on its <strong>latitude</strong> (Y coordinate) and <strong>longitude</strong> (X coordinate).  Since we aren’t programming on a phone, we’ll have to manually input the coordinates.  The command searches *all* stations for those that are &lt;= the specified distance, which is in given in miles.  If no stations are found, then “none found” is output.  Otherwise, the stations are output in sorted order, closest first.  If 2 stations are the same distance away, then output in order by station ID.  Feel free to use the built-in std::sort function.




For simplicity, you may assume valid user input with a single space between each value.  You can use the C++ stringstream object (much like parsing a CSV file) to obtain the individual values in the input.




As noted earlier, a function <strong>distBetween2Points(lat1, long1, lat2, long2)</strong> is provided to compute the distance between two points (lat1, long1) and (lat2, long2).  See the top of page 2 for a discussion of how to use this function in your program.  It turns out that you may get slightly different results based on the order in which you pass the coordinates to this function.  To match the screenshot above, the coordinates entered by the user should be the first coordinate (lat1, long1) passed to the function.




This also presents an interesting design question, because to implement this command you need to search

*all* the stations in the hashmap.  But the given hashmap class does not provide a mechanism for doing this; you need a station’s ID or abbreviation to find its data.  Think about how to extend the hashmap class, in the easiest way possible, to enable the main program to access all the stations.  What you cannot do is simply make the underlying hash table public, that’s too easy and goes against the notions of abstraction that we have been trying to build upon.  And you cannot move the “search for nearby stations” into the hashmap class, because the hashmap is meant to be general-purpose — the class is being used for a variety of different purposes, so a station-specific search function makes no sense in a general-purpose templated class.  You also cannot copy-paste the hashmap class to make one that is station-specific.




Instead, think about ways to provide general access to the elements of a hashmap.  With trees, we added <strong>begin()</strong> and <strong>next()</strong> functions to walk through the tree and provide access; maybe something similar could be done here?  Or perhaps you could implement an iterator class as we discussed in class, and allow foreach-like iteration through the hashmap?  Or in the simplest form, how about a function that returns a vector of all the keys in the hashmap?  One could then iterate through the vector and call search() to obtain the values.




<h2>Command #6:  find similar trips based on lat / long</h2>

Given a trip from station S to station D, this command searches and counts how many trips are similar to this one — i.e. start at S or a station nearby S *and* finish at destination D or a station nearby D.  Visually, we are counting the # of trips that fall within the circles below:







One possible algorithm is to find the set of stations S’ that include D and those nearby S.  Likewise, find the set of stations D’ that include D and those nearby D.  Now search all the trips and find those that start from a station in S’ and end with a station in D’.  Based on your choice of data structures, it should be possible to implement this algorithm in time O(S + T), where S is the # of stations and T is the # of trips.  Feel free to

use the built-in set or map data structures, or perhaps your hashmap.  [ <u>NOTE</u>: if you end up with doubly or triply-nested loops to solve this, you are on the wrong track. ]

For simplicity, you may assume valid user input with a single space between each value.  You can use the C++ stringstream object (much like parsing a CSV file) to obtain the individual values in the input.  It is possible for the user to input a trip that doesn’t exist, in this case output “no such trip”.










To match the output shown in the screenshot, the coordinates from the stations involved in the specified trip — the coordinates of S and D — should be the first coordinates (lat1, long1) passed to the <strong>distBetween2Points()</strong> function.

<h2>Efficiency</h2>

To test the efficiency of some of your code, a bigger trips file is provided in “trips2016-q1.csv”.




Given the amount of data, you may want to compile your program with optimization enabled — this can dramatically reduce execution time (from minutes to seconds in some cases).  To optimize with g++, add the option -O2 to your makefile:




g++ -O2 -std=c++11 main.cpp …




If you are working in Visual Studio, switch from “Debug” to “Release” mode using the toolbar (and then run without debugging).































<h2>Programming notes</h2>

Since the commands may contain multiple words, use the <strong>getline()</strong> function for all console input.  Also use getline() to input the filenames initially.  Here’s a nice trick to avoid having to type the filenames over and over again when testing (instead you can just press enter when prompted):




string stationsFilename = “stations.csv”; string tripsFilename = “trips.csv”; string filename;




cout &lt;&lt; “Enter stations file&gt; “;

<strong>getline(cin, filename);</strong>  // user can type a filename, or just press ENTER




if (filename != “”)      // user entered a filename, use it    stationsFilename = filename;




What’s the best strategy for success?  Build and test in stages.




<ol>

 <li>Finish part 1 with a working hashmap that you have tested.</li>

 <li>Start to build main(), and input the stations data. Do some searches and make sure it’s working, both by id and abbreviation.  Compare results against the input file.</li>

 <li>Now input the trips data, do some searches, and compare again the input file.</li>

 <li>Extend the trip input code to accumulate the bike usage stats.</li>

 <li>Add each command one by one, in the order shown.</li>

</ol>




The Gradescope submission scripts will grade the program based on the # of commands you have working, so you don’t need to complete the entire program to obtain a passing score.




<h2>Requirements</h2>

<ol>

 <li>The hashmap class must use hashing, and probing. You must use this class to store the data from the stations and trips input files, and compute the bike usages.  You are free to extend the hashmap class from part 1.  However, the hashmap class must remain general-purpose.  Do *not* add problem-specific data or functions to the class — no DIVVY-related structures or functions may be added to the hashmap class (this is discussed further on pages 5-6).  Also, do not simply make everything in the class public so the main() program can gain access to the underlying hash table directly.</li>

 <li>There can be only one hashmap class. Do not copy-paste and create multiple hashmap classes (one for stations, one for trips, one for bikes).  You don’t need to do this, that’s why the class is templated.</li>

 <li>You can use the built-in vector, map and set abstractions to help implement the main program, but not the unordered_map or its variants. However, the storing of all DIVVY data — bikes, stations, and trips — must be done using hashmap.  You’ll need to do some sorting, feel free to use the built-in sort algorithm.</li>

 <li>You cannot use the built-in hash functions, write your own hash functions to use with your hashmap class. Since you will have 4 hashmaps, you’ll need to write 4 hash functions.</li>

 <li>You can only open the input files, and read the data at most once; additional operations involving the data must involve memory-based data structures.</li>

 <li>You are required to free all memory allocated by your class. Valgrind will be used to confirm that memory has been properly freed.  Note that Codio may have a version of valgrind installed that reports a memory leak; see Piazza post @1707 for more details.</li>

 <li>No global variables.</li>

 <li>If any of the requirements are broken, we reserve the right to score the entire program as a 0.</li>

</ol>

<h2>Grading, electronic submission, and Gradescope</h2>

<strong><u>Submission</u></strong>:  all .cpp and .h files required to compile your program




Your score on this project is based on two factors:  (1) correctness as determined by Gradescope, and (2) manual review of program files for commenting, style, and approach (e.g. adherence to requirements).  Since part 2 is more complex than part 1, it is worth 100 points for correctness, and 40 points for manual review of commenting, style and overall approach / efficiency.  In this class we expect all submissions to compile, run, and pass at least some of the test cases; do not expect partial credit with regards to correctness.  <u>Example</u>:  if you submit to Gradescope and your score is a reported as a 0, then that’s your correctness score.  The only way to raise your correctness score is to re-submit.




In this project, your program will be allowed <strong>12 free submissions</strong>.  After 12 submissions, each additional submission will cost 1 point.  <u>Example</u>: suppose you score 100 after 15 submissions, and you activate this submission for grading.  Your autograder score will be 97:  100 – 3 extra submissions.  Note that you cannot use another student’s account to test your work; this is considered academic misconduct because you have given your code to another student for submission on their account.




By default, we grade your <strong><u>last</u></strong> submission.  Gradescope keeps a complete submission history, so you can <strong><u>activate</u></strong> an earlier submission if you want us to grade a different one; this must be done before the due date.  We assume *every* submission on your Gradescope account is your own work; do not submit someone else’s work for any reason, otherwise it will be considered academic misconduct.

<h2>Policy</h2>

Late work *is* accepted.  You may submit as late as 24 hours after the deadline for a penalty of 10%.  After 24 hours, no submissions will be accepted.




All work submitted for grading *must* be done individually.  While we encourage you to talk to your peers and learn from them (e.g. your “iClicker teammates”), this interaction must be superficial with regards to all




work submitted for grading.  This means you *cannot* work in teams, you cannot work side-by-side, you cannot submit someone else’s work (partial or complete) as your own.  The University’s policy is available here:




<a href="https://dos.uic.edu/conductforstudents.shtml">https://dos.uic.edu/conductforstudents.shtml</a> .




In particular, note that you are guilty of academic dishonesty if you <u>extend or receive any kind of</u> <u>unauthorized assistance</u>.  Absolutely no transfer of program code between students is permitted (paper or electronic), and you may not solicit code from family, friends, or online forums.  Other examples of academic dishonesty include emailing your program to another student, copying-pasting code from the internet, working in a group on a homework assignment, and allowing a tutor, TA, or another individual to write an answer for you.  It is also considered academic dishonesty if you click someone else’s iClicker with the intent of answering for that student, whether for a quiz, exam, or class participation.  Academic dishonesty is unacceptable, and penalties range from a letter grade drop to expulsion from the university; cases are handled via the official student conduct process described at <a href="https://dos.uic.edu/conductforstudents.shtml">https://dos.uic.edu/conductforstudents.shtml</a> .




<em>Page </em>