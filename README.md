# Lab Assignment 2 (Bonus Assignment)

For this lab assignment, you will correct and improve your solutions to Lab Assignment 1.  All points earned from this assignment will be considered bonus points.  You will test your API and fix any bugs.  If you lost any points on the first assignment, you will be able to recouperate the lost points by fixing the faulty methods (you will receive your Assignment 1 grade by Wednesday).  Therefore, you have the opportunity to earn perfect score on Asssignment 1.  In addition to fixing any bugs, you will be to earn additional bonus points by improving the style of your code.  This involves modifying your code so that it adheres with the [Google Java Style Guide](https://google.github.io/styleguide/javaguide.html). You should also change the names of your variables so that they are more descriptive.

## Goal
In this Lab Assignment you will learn to:

1. Create and run Unit tests
2. Use the debugger 
3. Write cleaner code

## Main Steps
1. Import the code from github directly into Eclipse.
2. Replace the ERDataReader.java and ERDataAnalyzer.java files with the versions you wrote for Assignment 1
3. Run the ERDataReaderTest unit test. Verify your ERDataReader is working properly. Fix your ERDataReader class if it is not working properly.
4. Finish implementing the ERDataAnalyzerTests. Fix any ERDataAnalyzer methods which are not working.
5. Polish your code
6. Compress the entire assignment2 folder into a .zip or .tar file and submit it as Assignment 2 on Ecampus

### Deadline
This assignment is due next week before lab. You may continue to work with your partner.

## 1. Clone the repository into Eclipse
Eclipse has the ability to import Java Projects directly from a github repository. 

1. Open Eclipse and click File -> Import
2. Select Git -> Projects from Git. Click Next
3. Select Clone URI. Click Next.
4. In the URI field, paste ```https://github.com/wvu-cs111/spring2017.git``` and then click Next.
5. For Branch Selection, ensure the ```master``` branch is selected and click Next.
6. Choose an empty directory for the repository.  This is where the spring2017 repository will be copied. The directory you choose must be empty (you cannot use the cs111_assignments directory we had been previously using). The name of the directory does not need to be ```spring2017```. You can name the repository whatever you wish. If you choose a directory which does not exist, Eclipse will create it for you. For example, you could enter:
   ```
   code/cs111/lab_assignments
   ```
   After choosing an empty directory, click Next.
7. Select Import existing Eclipse Projects and click Next.
8. You should see Eclipse has automatically found and selected the assignment2 project for import. Click Finish.

## 2. Replace the er_data modules
The ERDataAnalyzer.java and ERDataReader.java files in assignment2/src/er\_data are the incomplete files from Assignment 1.  Replace these files with the versions you submitted. 
1. Right-click on the er_data package under the assignment2/src folder in the Eclipse Package Explorer and select Import
2. Choose General -> File System and click Next.
3. Click Browse and navigate to the directory containing your ERDataAnalyzer.java and ERDataReader.java files then click OK.
4. Make sure the ERDataAnalyzer.java and ERDataReader.java files are checked in the Import File System window. Click Finish.
5. Confirm you want to overwrite the existing files with the ones you are importing.

If you do not have the code you submitted, you can download it from Ecampus by going to the Assignment 1 folder on the Ecampus website.

## 3. Unit tests
As you develop a program, you will frequently need to verify that it is working properly. Performing these tests manually is unreliable and inefficient. A much better approach is to write a suite of Unit Tests.  Whenever you change your code, you can run these unit tests to quickly check your program's behavior.

Notice that in the assignment2 project, we do not have the main program we were previously using to run our code. That's fine, we don't need it.  Instead, we'll use Java's unit testing library called JUnit to validate our code.

First let's test our readData() method. To do this, we will read in data from a test file whose values can be easily verified. The generateTestData() function produces a 3d array of values which should be equivalent to the 3d array returned by the readData() method, _if readData is working properly_. The unit test compares these two arrays to see if they are equivalent.

```java
// imports

public class ERDataReaderTest {

	// Data file containing the same values generated by generateTestData()
	private static final String TEST_DATA_FILE = "data/test_data.txt"; 
	
	private static int[][][] generateTestData() {
		int[][][] data = new int[DEFAULT_WEEKS][DEFAULT_DAYS][DEFAULT_HOURS];
		
		for (int week = 0; week < DEFAULT_WEEKS; week++) {
			for (int day = 0; day < DEFAULT_DAYS; day++) {
				for (int hour = 0; hour < DEFAULT_HOURS; hour++) {
					data[week][day][hour] = week + day + hour;
				}
			}
		}
		return data;
	}
	
	@Test
	public void testReadData() 
			throws FileNotFoundException, NoSuchElementException, IllegalStateException, IOException, Exception {
		
		int[][][] expecteds = generateTestData();
		int[][][] actuals = ERDataReader.readData(TEST_DATA_FILE);
		
		assertArrayEquals(expecteds, actuals);
	}

}
```
If we run this test class, JUnit will report whether or not our readData() method has returned the expected output and give us information if anything went wrong.

Once we have verified we are reading data correctly, we can start to test our ERDataAnalyzer. Here is an example test class which tests one method of the ERDataAnalyzer class:

```java
// imports
public class ERDataAnalyzerTest {
	@Test
	public void testPatientsPerWeek() {
		int[] expecteds = {3118, 2746, 2921, 2676};
		int[] actuals = ERDataAnalyzer.patientsPerWeek(TEST_DATA);
		assertArrayEquals(expecteds, actuals);
	}
}
```

Now you need to implement the remaining tests.  Fix any methods which fail to pass the test. The expected values of each method are provided in the next section. In the case of any methods which return decimal values (such the averaging methods), you must modify the assertArrayEquals method so that the test will pass if the difference between the decimal values are minimal.
```java
double delta = 0.00001;
assertArrayEquals(expecteds, actuals, delta)
```


### Answers to Assignment 1

In order to check your API, here is the correct output for each ERDataAnalyzer method:

```java
// patientsPerWeek
int[] answer = {3118, 2746, 2921, 2676};

// patientsPerDayPerWeek
int[][] answer = {{476, 537, 421, 362, 461, 431, 430},
		 {371, 362, 461, 349, 395, 486, 322},
		 {431, 531, 395, 383, 313, 434, 434},
		 {476, 461, 454, 265, 272, 348, 400}};

// averagePatientsPerWeek
double[] answer = {445.42857142857, 392.2857142857, 417.2857142857, 382.2857142857};

// averagePatientsPerDayAcrossWeeks
double[] answer = {438.5, 472.75, 432.75, 339.75, 360.25, 424.75, 396.5};

// busiestDayPerWeek
int[] answer = {1, 5, 1, 0};

//leastBusyDayPerWeek
int[] answer = {3, 6, 4, 3};
```

### Alternative testing
Perhaps we do not want to wait until we have a functioning readData method before testing ERDataAnalyzer. In this case we have a few options.
1. Make generateTestData() from our ERDataReaderTest class public. We could use this data since we know what the values are that get generated. However, if we use this approach, we still don't necessarily know what the correct output should be of the ERDataAnalyzer functions - we would only know what the input data is.

2. Fabricate simpler test data.  Instead of calculating statistics on a 4 x 7 x 24 array, we could create data for a 2 x 2 x 2 array, for example.  It is much easier to determine the expected output from a smaller array.  However, if we take this approach, then we must alter our ERDataAnalyzer methods to operate on 3d arrays of _any_ size (which is something you should probably do anyway).

## 4. Debugging
If one of the unit tests fails, we can use Eclipse's built in Java debugger to find the source of the error.  Using the debugger is far superior to using print statements or mentally thinking through the code.

Place a breakpoint in the method which is causing the failure. You can place a breakpoint by double left-clicking in the margin to the left of the code. This is the same column where the line numbers appear (if you have line numbers set to be visible).

Once you have set a breakpoint, you can re-run the test in debug mode by clicking on the bug-looking button at the top of the window, near the Run button. Eclipse may warn you if your file contains errors when you try to run the debugger.  Continue anyway, and confirm that you want to change your perspective to the Debug perspective.

Your program will now run up to the point of the breakpoint and then pause its execution. At this point you can "Step Over" (hotkey is F6) to begin executing one line of code at a time, or you can "Step Into" (F5) to enter into any methods at the current line in order to see what is going on "underneath the hood."

## 5. Clean coding
In this assignment, bonus points will be awarded for code style, organization, and readability. Particularly:

1. Adhering to the [Google Java Style Guide](https://google.github.io/styleguide/javaguide.html)
  * Pay special attention the following style guidelines:
    * [Whitespace](https://google.github.io/styleguide/javaguide.html#s4.6-whitespace)
    * [Naming Conventions](https://google.github.io/styleguide/javaguide.html#s5.2-specific-identifier-names)
    * [Column Limits](https://google.github.io/styleguide/javaguide.html#s4.4-column-limit)
    * [Indentation](https://google.github.io/styleguide/javaguide.html#s4.2-block-indentation) 
2. Choosing good variable names
3. Avoiding code duplication

## 6. Submit the assignment
In Ubuntu, right-click on the assignment2 project folder. Select Compress. Submit the assignment2.tar.gz file.
In Windows or Mac, you can zip the assignment2 project folder and submit that.

Submit the compressed project folder on Ecampus.
