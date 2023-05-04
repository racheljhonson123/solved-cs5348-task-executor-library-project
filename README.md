Download Link: https://assignmentchef.com/product/solved-cs5348-task-executor-library-project
<br>
<h1>Installing Java and Eclipse IDE</h1>

This document and the following video assumes that you have both Java runtime and the Eclipse IDE installed on your PC. If this is not the case the following link is to a video that does a nice job of explaining the process. There are many other videos and other resources to be found on the web. Let me know if you need help.

The Java / Eclipse Video: <a href="https://youtu.be/70dN5jqumAs">https://youtu.be/70dN5jqumAs</a>

<h1>Project YouTube Video</h1>

A YouTube video has been provided that demonstrates the installation of the development project and testing project in an Eclipse workspace. The video also demonstrates the execution of the testing project and how to export the development project as <strong>a jar file</strong> to be submitted for grading.

The video URL is: <a href="https://youtu.be/DEvkzMk4BNA">https://youtu.be/DEvkzMk4BNA</a>

<h1>Project Description</h1>

This project will produce <u>library</u> JAR file containing a TaskExecutor service. See the video and the section “Packaging the Library JAR File” for more details.

TaskExecutor is a service class that maintains a pool (collection) of N threads <a href="https://en.wikipedia.org/wiki/Thread_pool">(</a><a href="https://en.wikipedia.org/wiki/Thread_pool">See Thread Pool</a> <a href="https://en.wikipedia.org/wiki/Thread_pool">Design Pattern</a><a href="https://en.wikipedia.org/wiki/Thread_pool">)</a> that are used to execute instances of Tasks provided by the TaskExecutor’s clients. Interfaces for both the TaskExecutor and Task have been provided and must be used to implement the service (<u>including the package structure</u>).

You have also been provided a driver application and Task implementation

(TaskExecutorTest.java and SimpleTestTask.java) that you will need to use to design, debug, and test your submissions.

<h1>Project Goals</h1>

The <strong>primary goal</strong> of this project is to have teams <u>implement the multithreaded synchronization</u> <u>needed to implement the TaskExecutor service</u>.

The <strong>secondary goal</strong> of the project (but the most difficult) is to <u>implement a Finite Bounded</u>

<u>FIFO Buffer</u> described in the textbook, and in class, using only Java’s Object as monitors and <strong>without the use of synchronized methods</strong>. <u>Note</u> Synchronized blocks are acceptable, even needed to implement the bounded buffer.

<h1>Goal1: TaskExecutor Service</h1>

The TaskExecutor is a service that accepts instances of Tasks (classes implementing the Task interface) and executes each task in one of the multiple threads maintained by the thread pool<u>.</u> <u>That is, the service maintains a pool of <strong>pre-spawned threads</strong> that are used to execute Tasks.</u>

Figure 1 provides an overview of the structure and design of the TaskExecutor service. Clients provide implementations of the Task interface which performs some application-specific operation. Clients utilize the TaskExecutor.addTask() method to add these tasks to the Blocking FIFO queue. Pooled threads remove the tasks from the queue and call the Task’s execute() method. This Task executes for some amount of time before completing by returning from the execute() method. At this point the task-running threads attempts to obtain a new Task from the queue. If the blocking queue is empty (no tasks to execute) the task running <u>thread’s execution</u> <u>must be blocked until a new task is added to the task queue</u>. If the blocking queue is full (no space to add a new task) the <u>client’s thread’s execution must be blocked until a task is been</u> <u>removed from the queue</u>.

<strong>Figure 1: Task Executor Overview </strong>

<h1>Goal 2: Implementing the Blocking Queue</h1>

Teams are to provide an implementation of a Blocking Queue. This is a FIFO queue that is both thread-safe and blocking. Because the queue is internal to the TaskExecutor, the project does not specify the queue’s interface. However, for the sake of discussion let us use the following interface as an example:

public interface BlockingFIFO

{

void put(Task item) throws Exception;

Task take() throws Exception;

}

By ‘Blocking’ we mean that…

<ol>

 <li>The put(task) method places the given task into the queue. If the queue is full, the put() method must blocking the client’s thread until space is made available i.e. when a Task is removed from the queue through the take() method.</li>

 <li>The take() method removes and returns a task from the queue. If the queue is empty, the pooled thread calling take() will block until a Task has been placed in the queue though the put() method.</li>

</ol>

As described in the Grading Section, teams have two options when implementing the Blocking Queue.

<ol>

 <li>Teams can initially use the class <a href="https://docs.oracle.com/javase/7/docs/api/java/util/concurrent/ArrayBlockingQueue.html">util.concurrent.ArrayBlockingQueue</a> which is provided by the Java Development Kit (JDK) runtime library.</li>

</ol>

ArrayBlockingQueue implements the needed blocking behavior as described above. But the use of ArrayBlockingQueue provides less than full credit for the project.

<ol start="2">

 <li>Teams implement their own BlockingFIFO queue. The <u>three</u> restrictions are 1) teams must implement their BlockingFIFO using the boundedbuffer design as provided in the book (and later in this document) 2) the BlockingFIFO must implement using Java Objects as monitors (Not Semaphores) as described in this document and, 3) implementations must be based on using an array of Task as its container. That is, the size of the queue must be fixed when the container is created. The FIFO implementation size (array length) must be no more than 100 elements. Implementations cannot use any of Java’s built-in container classes (e.g. ArrayList) which has its built-in synchronization.</li>

</ol>

<h1>Project Requirements</h1>

<u>Be sure to read and understand these project requirements. Your submissions will be held</u> <u>accountable for them.</u>

<ol>

 <li><strong>The BlockingFIFO must implement the design given for boundedbuffer as described in the text book, and later in this document (See the section BlockingFIFO Implementation Notes). Any other FIFO design will not receive credit for the implementation. </strong></li>

 <li><strong>The BlockingFIFO must implement its synchronization using Java Objects as monitors. </strong><strong>You cannot use Semaphores, Locks, or other synchronization mechanisms offered by Java. </strong></li>

 <li><strong>The classes implementing the TaskExecutor, BlockingFIFO, </strong><strong>Thread Pool cannot contain synchronized methods. All of the synchronization must be implemented using primitive Object monitors. Also synchronized block will be needed. </strong></li>

 <li>The project will provide an implementation of the provided TaskExecutor interface.</li>

 <li>The TaskExecutor will accept and execute implementations of the provided Task interface.</li>

 <li>The TaskExecutor and Task interface must implement the interface definitions given in the student development project source files <u>including package and exact method</u> <u>signatures</u>e. no changes to the interfaces are allowed.</li>

 <li>You have been provided with the source for the test program TaskExecutorTest and the task SimpleTestTask both of which you can use to exercise and verify your TaskExecutor implementations. <u>However, you must not modify either of these test</u> <u>classes</u>. <strong><em><u>Grading will be evaluated using unmodified versions of these test</u> <u>classes</u></em></strong>.</li>

 <li>Each thread will be assigned a unique name when created. See Thread.setName(String).</li>

 <li>Threads will be maintained in a pool and reused to execute multiple Tasks. Threads <u>are</u> <u>not</u> to be created and destroyed for each task’s execution.</li>

 <li><u>Exceptions thrown during Task execution cannot cause the failure of the executing</u> <u>thread</u>. It is suggested that Task execution be wrapped in a try / catch block that logs and ignores the caught exceptions.</li>

 <li>Tasks will execute concurrently on <strong>N threads</strong> where N is <strong>the thread pool size</strong> and is provided as a service initialization parameter.</li>

 <li>The BlockingFIFO implementation must use an Array to store tasks. You cannot use synchronized data structures such as ArrayList to implement your blocking queue.</li>

 <li><em>The </em>BlockingFIFO <em>implementation must </em><em>use an array of length no more than 100 elements.</em> The reason for this requirement is that an array larger that the number of tasks injected during testing will not exercise the blocking nature of FIFO put() operations.</li>

 <li>When the number of inserted tasks exceeds the number of threads, unexecuted tasks will remain on the BlockingFIFO until removed by a task running thread.</li>

 <li>Every pooled thread’s execution must block when the BlockingFIFO is empty e. Pool threads should not spin or busy-wait when attempting to obtain a task from the service’s empty FIFO.</li>

 <li>When the BlockingFIFO is full, client threads attempting to add a new task to the queue must block until a Task is removed. Again, no polling, or busy waiting is allowed.</li>

 <li><strong>The project will be delivered as a library jar file which will be linked with the test applications used to initialize and test the TaskExecutor’s correct implementation. </strong></li>

 <li>The TaskExecutor should catch, report (log), and eat any exceptions thrown during an application-specific Task’s execution e. an <strong><u>Exception thrown by a Task should not</u> <u>cause a pool’ed thread to exit</u></strong>.</li>

 <li><strong><u>Your implementation cannot print any messages to stdout</u>. The number of lines printed to the console will be used to determine the correctness of your implementation and any additional lines of text will throw off the count and will cause your project to fail. </strong></li>

 <li>It is entirely correct for the test application to <u>not exit</u> once the N tasks have been processed. I will leave it to students to figure out the reason why &#x263a;.</li>

</ol>

<h1>Packaging the Library JAR File</h1>

Your implementation of the TaskExecutor and the interfaces will be packaged and delivered in a library JAR file. The implantation will be evaluated by executing a prewritten test application using the provided library jar. The TaskExecutor and Task interfaces must be delivered in the package specified by the given source files. Instructions for exporting your project into a library JAR file has been provided at the end of this document.

<h1>Student Testing</h1>

Teams have been provided a sample application and Task implementation

(SimpleTestTask.java and TaskExecutorTest.java) that they can use to test their TaskExecutor implementation. This code can be used to test your implementation. Remember that eventually your team needs to execute the test application / task against the library jar file that will be submitted for grading. In Eclipse, this means generating the jar in a development project and executing the test application using the imported JAR on its classpath.

<h1>Instructor Testing</h1>

Team will submit for grading a <u>library JAR file</u> containing their implementation of TaskExecutor interface. The library jar will be installed in a project containing a test application

SimpleTestTask.java and TaskExecutorTest.java. It is expected that 1) the test program compile without any modification 2) that the program TaskExecutorTest.java produces the correct result from the execution of Task implementations that are part of the testing procedure.

<strong>NOTE</strong>: As explained in class, one success criteria will be counting the number of output lines generated by TaskExecutorTest.java. Your implementation can not modify either

SimpleTestTask.java or TaskExecutorTest.java. Your implementation submited for grading cannot produce any console output<u>. You can (and should) use console output for debugging,</u> <u>but be sure that the additional output is commented out or removed from your code before</u> <u>compiling and packaging your library JAR file for submission</u>.

<h1>Application Output</h1>

The output generated by the TestExecutorTest application is useful for diagnosing the correct operation of the TaskExecutor’s implementation. The project materials provided include a file “sampleOutput.txt” which illustrates the following points:

<ol>

 <li>Initially there will be N+M task injection messages printed to the output where N is the size of the FIFO and M is the number of threads.</li>

 <li>The output will then vary from messages produced by executing threads and messages produced by the injection of new tasks into the queue.</li>

 <li>Tasks names (e.g. SimpleTask234) will be printed in non-sequential order indicating that the tasks are being schedule out of insertion order (as should be expected).</li>

 <li>Thread names (e.g. TaskThread8) should also be listed in a random order.</li>

 <li>The numberOfActivations counter should be equal the number of tasks at the end of the run.</li>

 <li>The number of the lines in the file should equal 2 times the number of tasks.</li>

</ol>

<h1>Provided Materials</h1>

Teams have been provided the following materials on eLearning.

<ol>

 <li><u>Task Executor Project Description.docx</u>: This document.</li>

 <li><u>txt</u>: This is a sample of the correct output generated from the testing application and task provided in the project taskExecutorStudentTestingProject.</li>

 <li><u>Task Executor Evaluation – Team XX.docx</u>: This document is to be completed by teams and turned in with the other project deliverables.</li>

 <li><u>zip</u>: This is an Eclipse project that can be imported into an Eclipse workspace. It serves as the foundation / starting point for the team’s development efforts. See the section “Importing a Project from a Zip File” for instructions on importing projects into your Eclipse workspace.</li>

 <li><u>zip</u>: This is an Eclipse project that can be imported into an Eclipse workspace. It contains the testing code that will be executed against team’s implementations. See the section “Importing a Project from a Zip File” for instructions on importing projects into your Eclipse workspace.</li>

</ol>

<h1>Materials to be Submitted using a UTD Box Folder</h1>

The following items must be placed in a UTD Box Folder used to submit your projects. See <a href="https://utdallas.account.box.com/login">https://utdallas.account.box.com/login</a> .

After you create the Box folder, you must share the folder with <u><a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="e5888286d5d4d6d5d5d5a59091818489898496cb808190">[email protected]</a></u> (me!) and make my account an Editor so I can both read from, and place files into your project folder.

Once the folder is created, place these materials into it for grading…

<ol>

 <li>The completed evaluation document “Task Executor Evaluation – Team XX” with the XX replaced by the team’s number and the names and NetIDs of team members.</li>

 <li><u>All project source code</u>. The simplest approach is to zip and submit your Eclipse project taskExecutorStudentTestingProject.</li>

 <li>A library JAR containing the Task and TaskExecutor interfaces, their TaskExecutor implementation, and any additional classes needed to support their implementation. The library JAR must compile and execute against an unmodified SimpleTestTask.java and TaskExecutorTest.java as provided including maintaining the package structure.</li>

</ol>

<h1>Grading Criteria</h1>

The following is the criteria and percentages used to assign the project’s grade.

<ol>

 <li>The test program produces the correct results b<u>ut uses java.util.concurrent.BlockingQueue</u> <u>to implement the BlockingFifoQueue</u>: <strong>60 pts</strong>.</li>

 <li>The test program produces the correct results and the <u>team implements their own</u> <u>BlockingFifoQueue as described in this document</u>. : <strong>40 pts</strong>.</li>

</ol>

<h1>Class Interfaces</h1>

The following are the interfaces provided in the source files TaskExecutor.java and Task.java. These interfaces must be implemented and the code must compile and execute against the test

code provided under the eLearning folder TestingSource (SimpleTestTask.java and TaskExecutorTest.java).

<strong>public</strong> <strong>interface</strong> Task

{

<strong>    void</strong> execute();     String getName();

}

<strong>public</strong> <strong>interface</strong> TaskExecutor

{

<strong>    void</strong> addTask(Task task);

}

<h1>BlockingFIFO Implementation Notes</h1>

Our text book provides a description of Bounded Buffer on page 229 and shown below as Java pseudo code which must serve as a start of your team’s BlockingFIFO’s design<u>. Note that this</u> <u>design utilizes monitors to implement thread blocking.</u>

// Producer Consumer Example from Bounded Buffer on page 229  // translated to Java-like pseudo code.




Task buffer[N]; int nextin, nextout; int count;

Object notfull, notempty; // Monitors used for synchronization

void put(Task task)

{   if(count == N) notfull.wait();   // Buffer is full, wait for take

synchronized(this) {     buffer[nextin] = X;     nextin = (nextin + 1) % N;     count++;     notempty.notify(); // Signal waiting take threads

}

}




Task take()

{

if(count == 0) notempty.wait(); // Buffer is empty, wait for put   synchronized(this) {

Task result = buffer[nextout];     nextout = (nextout + 1) % N;     count–;     notfull.notify(); // Signal waiting put threads     return result;

}

}

<h2>Important Note</h2>

Be aware that this design contains a race condition that needs to be addressed in your BlockingFIFO implementation. The race condition exists between threads that are unblocked by the notEmpty / notFull monitors and the other concurrently executing threads.

This is an example of the race condition / problem that exist with the design shown above:

<ol>

 <li>Thread 1 enters append(), finds the queue full (count == N), and waits on the monitor notFull.</li>

 <li>Thread 2 enters take(), removes an item from the queue, and signals (notify) monitor notFull.</li>

 <li>At this point Thread 1 is unblocked and eligible for scheduling. <u>However, this does not</u> <u>mean that Thread 1 immediately begins execution</u>. At this point the unblocked thread is</li>

</ol>

“ready” to be scheduled for execution.

<ol start="4">

 <li>Before Thread 1 is scheduled for execution, <strong>Thread 3</strong> executes append(), finds count &lt; N, and adds an item to the queue. <u>Now count = N</u>.</li>

 <li><strong>Notice that when Thread 1 resumes execution, it does not re-check if count is still &lt; N (which it won’t be because of Thread 3’s action). </strong></li>

</ol>

It is up to your team to create a solution, or an alternative design, that avoids this race condition while still retaining the BoundedBuffer design outlined in the text. As described in class, a synchronized block on ‘this’ is needed to allow the put / take methods to manipulate the array data structure without a race condition.

<h1>TaskRunner Design</h1>

A suggested design for the threads responsible for executing Tasks is to create a Runnable that

1) obtains a Task from the FIFO 2) executes the TASK by calling the execute() method (See

Figure 4). The following provides an example of the TaskRunner’s implication of its run() method.

public void run() {     while(true) {

// take() blocks if queue is empty         Task newTask = blockingFifoQueue.take();         try {             newTask.execute();

}         catch(Throwable th) {

// Log (e.g. print exception’s message to console)             // and drop any exceptions thrown by the task’s

// execution.

}

}

}

Note that the execution of the task is wrapped in an exception handler that will consume any

Throwable generated during the execution of the Task’s execute method. This is needed to keep the Throwable (exception) from killing the TaskRunner’s thread.

The following is a sample design of this service. The interfaces provided by a Java library or by the project are marked in blue.  The Task implementation is also marked in blue. The remaining classes compose a suggested design implementing the project’s requirements.




<strong>Figure 2: Suggested Design </strong>







<strong>Figure 3: Suggested TaskExecutor Initialization </strong>




<strong>Figure 4: Suggested Task Runner Design </strong>

<strong> </strong>

<h1>Working With Eclipse Importing Projects from a Zip File</h1>

Note that the material presented in this section is better described in the project video mentioned at the beginning of this document.

Many programming assignments will provide an exported project that you can import into you Eclipse workspace. These projects will be provided zip file archives that will be one of the files that can be downloaded from eLearning. The zip archive may contain sample code or a project template that can be used as a starting point for your efforts. You will be importing the project zip archive into your workspace.

<strong>Optional: Removing existing projects with the same name from the workspace </strong>

You cannot import a project with the same name into the workspace. This means that if you import and try to re-import the project template you must first delete the old project from the workspace. This is accomplished by selecting the existing project from the package explorer and selecting the “Edit &gt; Delete” menu item. This will bring up the dialog shown in the following graphic. Notice that the option “Also Delete Contents Under C:…” is selected<strong>. It is very important that this option is selected</strong> so that the project files are removed from you workspace




A this point the Old project will be have been removed from your workspace and you may begin importing the project template

<h2>Importing the Project</h2>

The process for importing the template project is a follows.

Open the import wizard using the “File &gt; Import” menu item. This brings up the import dialog shown in the following graphic. Make sure to select the “Existing Projects into Workspace” option (under General) and press Next.




This brings up the following import dialog. There are a few import things to note:

<ol>

 <li><u>You need to select the “Select archive file” option</u> and then press browse to select the project template archive (zip) file.</li>

 <li>When the file opens, you need to select the project.</li>

 <li>Press Finish and the project will be imported into your workspace.</li>

</ol>




<h1>Exporting an Eclipse Project as a Library JAR File</h1>

Note that the material presented in this section is better described in the project video mentioned at the beginning of this document.

This section provides a procedure describing of how to export the project containing your team’s TaskExecutor implementation as a library .jar file for submission.

<ol>

 <li>Select the project that you wish to export.</li>

 <li>Use the right mouse button, or the file menu, to select the Export feature.</li>

 <li>Select Java &gt;&gt;JAR File as shown below, and then Next.</li>

</ol>




<ol start="4">

 <li>On the JAR Export panel, make sure that the desired project is selected and enter the path and file name for the exported library jar file.</li>

 <li>Select Finish and the export operation will be completed.</li>

</ol>





