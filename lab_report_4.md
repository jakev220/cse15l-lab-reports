# Lab Report 4 - Doing It All From The Command Line (Week 7)

In this [lab](https://ucsd-cse15l-s23.github.io/week/week7/#week7-lab-report), we were tasked with reproducing the tasks from the Lab 7 activities and recording our keystrokes. The tasks are as follows:

```
1. Setup Delete any existing forks of the repository you have on your account
2. Setup Fork the repository
3. The real deal Start the timer!
4. Log into ieng6
5. Clone your fork of the repository from your Github account
6. Run the tests, demonstrating that they fail
7. Edit the code file ListExamples.java to fix the failing test (as a reminder, the error in the code is just that index1 
   is used instead of index2 in the final loop in merge)
8. Run the tests, demonstrating that they now succeed
9.Commit and push the resulting change to your Github account
```

For the purposes of this lab report, I will only be reproducing tasks 4-9.

## 1) Logging In

// insert screenshot of step 4

To log into my student account, I typed the following in the command line:

```
$ ssh cs15lsp23bn@ieng6.ucsd.edu
```

Once I entered my password, I was logged in, as you can see in the screenshot above.


## 2) Cloning the Repository

In this step, I cloned a fork of the repository that was given in the lab instructions. I typed the following in the command line:

```
$ git clone https://github.com/ucsd-cse15l-s23/lab7
```

Typing this in the command line cloned the repository into my GitHub account, as shown in the screenshot below.

// insert screenshot of step 5


## 3) Running Tests

To run the tests, I used some code from [Week 3](https://ucsd-cse15l-s23.github.io/week/week3/). Before that, I typed `$ cd lab7` to get into the correct directory to run these tests. Once I was in that directory, I pasted the following code using `cmd - V`:

```
$ javac -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar *.java
$ java -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar org.junit.runner.JUnitCore ListExamplesTests

```

When the tests were ran, this is the error that was produced:

// insert screenshot of step 6


## 4) Debugging the Code in the Command Line

To edit the `ArrayExamples.java` file in the command line, I used the `vim` command. This is what I typed into the command line:

```
$ vim ListExamples.java
```

Typing that into the command line took me to this screen, shown below:

// insert screenshot of step 7

Here are the keys that were pressed to get to the point of the error:
`<up><up><up><up><up><up><right><right><right><right><right><right><right><right><right><right><right><right>`

Once I reached that point, I pressed the `<i>` key to enter insert mode. Then, I clicked `<backspace>` on `index1` and replaced the `1` with `2`. Then, I pressed the following keys:

`<esc>` to exit insert mode, and `:wq` to save and quit vim. 


## 5) Checking the Tests

To check the tests, I had to compile and run `ListExamplesTests.java` again. The keys I pressed were:

`<up><up><up><up><up>` to ` javac -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar *.java`, and then `enter`. The  `javac -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar *.java` was 5 up in the search history, so I used the up arrow to access it. Similarly, I accessed the `-cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar org.junit.runner.JUnitCore ListExamplesTests` the same way, as it was 5 up in the history.

With that being done, the tests were compiled, ran, and passed! Here is what was returned in the terminal:

// insert screenshot of step 8


## 6) Committing Changes and Pushing to Github

Now that the bug has been fixed and the tests have been passed, it's time to commit the changes and push them to my GitHub account. In the command line, I wrote the following commands:

```
$ git add .
$ git commit -m "changed index 1 to index 2 in final loop of merge method"
```

In the first command, I had to add all of my modified files, which in this case was just `ListExamples.java`. Once the modified file was saved, I could then add a commit message using the second command. My commit message describes exactly what changes I made to the code, for the purposes of being detailed. Once that was entered, my changes have been made!

Here is what appeared in my terminal after using these two commands:

//insert screenshot of step 9

---

Thank you for reading :)


