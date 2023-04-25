# Lab Report 2 - Servers and Bugs (Week 2)

In this [lab](https://ucsd-cse15l-s23.github.io/week/week3/#week3-lab-report), we created a web server that keeps track of a single string and
updates it according to incoming requests. We also debugged several programs
by writing test cases and fixing the code to eliminate the errors being produced.

## Part 1 - Creating a Web Server

In a file titled `StringServer.java`, I coded the following:

```
import java.io.IOException;
import java.net.URI;

class Handler implements URLHandler {
    String string = "";

    public String handleRequest(URI url){
        System.out.println("Path: " + url.getPath());
        if (url.getPath().equals("/")){
            return string;
        }

        else if (url.getPath().equals("/add-message")){
            String[] parameters = url.getQuery().split("=");
            if (parameters[0].equals("s")){
                string += (parameters[1]) + "\n";
                return string;
            }
        }
        return "404 Not Found!";
    }
}

class StringServer {
    public static void main (String[] args) throws IOException {
        if (args.length == 0){
            System.out.println("Missing port number! Try any number between 1024 to 49151");
            return;
        }

        int port = Integer.parseInt(args[0]);

        Server.start(port, new Handler());
    }
}
```

To write this program, I used files from the `wavelet` [GitHub repository](https://github.com/jakev220/wavelet)
as a reference for coding a web server. More specifically, I modified the `NumberServer.java` program to
keep track of a single string that gets modified by an incoming request that looks something like this:

`/add-message?s=<string>`

This request concatenates a new line `\n` and `<string>` to the string, and returns the updated string.

To run this program, I also needed to use the `Server.java ` file in the `wavelet` GitHub repository
to initialize a server that can be visited and modified. The server number is given by the user in the
command line, as seen in the `StringServer` class. The following block contains the contents of `Server.java`:

```
// A simple web server using Java's built-in HttpServer

// Examples from https://dzone.com/articles/simple-http-server-in-java were useful references

import java.io.IOException;
import java.io.OutputStream;
import java.net.InetSocketAddress;
import java.net.URI;

import com.sun.net.httpserver.HttpExchange;
import com.sun.net.httpserver.HttpHandler;
import com.sun.net.httpserver.HttpServer;

interface URLHandler {
    String handleRequest(URI url);
}

class ServerHttpHandler implements HttpHandler {
    URLHandler handler;
    ServerHttpHandler(URLHandler handler) {
      this.handler = handler;
    }
    public void handle(final HttpExchange exchange) throws IOException {
        // form return body after being handled by program
        try {
            String ret = handler.handleRequest(exchange.getRequestURI());
            // form the return string and write it on the browser
            exchange.sendResponseHeaders(200, ret.getBytes().length);
            OutputStream os = exchange.getResponseBody();
            os.write(ret.getBytes());
            os.close();
        } catch(Exception e) {
            String response = e.toString();
            exchange.sendResponseHeaders(500, response.getBytes().length);
            OutputStream os = exchange.getResponseBody();
            os.write(response.getBytes());
            os.close();
        }
    }
}

public class Server {
    public static void start(int port, URLHandler handler) throws IOException {
        HttpServer server = HttpServer.create(new InetSocketAddress(port), 0);

        //create request entrypoint
        server.createContext("/", new ServerHttpHandler(handler));

        //start the server
        server.start();
        System.out.println("Server Started! Visit http://localhost:" + port + " to visit.");
    }
}

```

After initializing the server through the terminal, the string that the server stores
was now able to be updated according to the commands provided in the web address. 
Here is an example of the server after adding a few messages:

// insert screenshot of localhost:4000

In this first screenshot, the `handleRequest(URI url)` method is called, and it satisfies the first condition.
For reference, this is the condition:

```
        if (url.getPath().equals("/")){
            return string;
        }
```
In this condition, the getPath() method, a method in the `File` class, is called on the `url` provided. According to user 
andrew1234 on [Geeks For Geeks](https://www.geeksforgeeks.org/file-getpath-method-in-java-with-examples/#)
the `getPath()` "returns the path of the given file object" and "returns a string object
which contains the path of the given file object." 

Then, java checks if the string returned by `getPath()` is equal to `/` using the built in `equals` method.
This method checks if two strings are equal to each other. If this condition is satisfied, the string stored in the Server
will be returned as it was before.

..but how did I edit the string to be as its shown in the screenshot?

Well, similar to before, the `handleRequest(URI url)` method is called, and the `url` provided satisfies the other condition,
which is:

```
 else if (url.getPath().equals("/add-message")){
            String[] parameters = url.getQuery().split("=");
            if (parameters[0].equals("s")){
                string += (parameters[1]) + "\n";
                return string;
            }
        }
```

As described before, Java checks if the string returned by the `getPath()` method is equal to `/add-message`
using the `equals` method. When this condition is satisfied, the `getQuery()` method is called on `url`.
According to user andrew1234 on [Geeks For Geeks](https://www.geeksforgeeks.org/uri-getquery-method-in-java-with-examples/),
the getQuery() method is "part of the URI class" and "returns the Query of a specified URL." 

Notice the `?` in the request provided:

`/add-message?s=<string>`

This symbol represents the start of the query, which are the contents of the request that follow it. 
This query is then split into a String list called `parameters` by the `=` symbol using the built in `split` method.

Then, within this request, Java checks if the first element in `parameters` is equal to s using the `equals` method again.
If this condition is satisfied, `string` is updated to be the current string concatenated with the second element in `parameters`
and the `\n` string, which creates a new line after the string.

Here is a screenshot of the server when the `/add-message` request is typed into the web address:

// insert screenshot of localhost:4000/add-message?s=<string>
  
Additionally, if the user makes a request that does not satisfy either of these conditions, it will return:

`404 Not Found!`
  
A value that is being changed when the add-message request is made is the `string` variable.
This is because the value that is updated and returned in the `Handler` class is `string`. The url value can be
updated through the request in the web address as well, since it is given by the user.


# Part 2 - Debugging

The other portion of this lab was concerned with finding test cases that make the program fail and debugging the program to make the test cases pass. 

The following two methods in a filed named `ArrayExamples.java` have bugs in them:

```
public class ArrayExamples {

    /* Changes the input array to be in reversed order */
    
    static void reverseInPlace(int[] arr) {    
        for (int i = 0; i < arr.length; i += 1) {
            arr[i] = arr[arr.length - i - 1];
        }
    }
    
    /* returns a *new* array with all the elements of the input array in
    reversed order */
    
    static int[] reversed(int[] arr) {
        int[] newArray = new int[arr.length];
        for (int i = 0; i < arr.length; i += 1) {
            arr[i] = newArray[arr.length - i - 1];
        }
        return arr;
    }
}
```

The following test cases in another file named `ArrayTests.java` are failure inducing inputs:

```
import static org.junit.Assert.*;
import org.junit.*;

public class ArrayTests {
    @Test
    public void reverseManyItems() {
        /* tests reverseInPlace method */
        int[] numList = {0, 10, 20};
        ArrayExamples.reverseInPlace(numList);
        assertArrayEquals(new int[]{20, 10, 0}, numList);
    }
    
    public void newArrayReversed() {
        /* tests reversed method */
        int[] numList = {5, 10, 15};
        assertArrayEquals(new int[] {15, 10, 5}, ArrayExamples.reversed(numList));
    }
}    
```

The following test cases, in the same file as the previous two, do not induce a failure:

```
import static org.junit.Assert.*;
import org.junit.*;

public class ArrayTests {
    @Test
    public void testReverseInPlace() {
        int[] input = {3};
        ArrayExamples.reverseInPlace(input1);
        assertArrayEquals(new int[]{3}, input1);
    }
    
    @Test
    public void testReversed() {
        int[] input1 = {};
        assertArrayEquals(new int[]{}, ArrayExamples.reversed(input1));
    }
}
```

The following screenshots showcase the output of running the tests, showing the symptom of the programs:

//insert screenshot 1
//insert screenshot 2

After identifying the program's bugs, I then proceeded to fix the code. The following code blocks show the code before and after debugging:

```
// BEFORE DEBUGGING
public class ArrayExamples {

    /* Changes the input array to be in reversed order */
    
    static void reverseInPlace(int[] arr) {    
        for (int i = 0; i < arr.length; i += 1) {
            arr[i] = arr[arr.length - i - 1];
        }
    }
    
    /* returns a *new* array with all the elements of the input array in
    reversed order */
    
    static int[] reversed(int[] arr) {
        int[] newArray = new int[arr.length];
        for (int i = 0; i < arr.length; i += 1) {
            arr[i] = newArray[arr.length - i - 1];
        }
        return arr;
    }
}
```

```
// AFTER DEBUGGING
public class ArrayExamples {

  /* Changes the input array to be in reversed order */
  
    static void reverseInPlace(int[] arr) {
        int[] arrCopy = arr.clone();
        for(int i = 0; i < arr.length; i += 1) {
            arr[i] = arrCopy[arrCopy.length - i - 1];
        }
    }

  /* Returns a *new* array with all the elements of the input array in reversed order */
  
    static int[] reversed(int[] arr) {
        int[] newArray = new int[arr.length];
        for(int i = 0; i < arr.length; i += 1) {
            newArray[i] = arr[arr.length - i- 1];
        }
        return newArray;
    }
}
```

The initial `reverseInPlace(int[] arr)` method was failing because the for loop kept changing the elements and updating to the already updated elements of the same list. The solution to this bug was to make a copy of the list and update the list according to the copy.

The initial `reversed(int[] arr)` method was failing because it was updating the array given instead of updating the new array. The solution to this bug was to update the new array index to be the updated index of the original array.


# Part 3 - Reflection

*In a couple of sentences, describe something you learned from lab in week 2 or 3 that you didn't know before.*

In week 2 and in this lab report, I learned how to write a web server using java code. Previously, my only experience with coding a website was with HTML and CSS. Being able to see what code is needed to initialize a server and how to write code to take requests at the web address line was super interesting and made me wonder how this can be applied to more advanced web applications. Initially, I struggled to conceptualize how web servers worked and had a hard time coding the `StringServer.java` program. However, with the assistance of the CSE 15L instruction team, I was soon able to code a web server and make it actually work according to the instructions. 

--- 

Thank you for reading! :)
