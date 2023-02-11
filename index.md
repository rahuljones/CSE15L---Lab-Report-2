# Lab Report 2

**Part 1: String Server**
This is my code for the StringServer:
```
import java.io.IOException;
import java.net.URI;
import java.util.ArrayList;

class Handler implements URLHandler {
    // The one bit of state on the server: a number that will be manipulated by
    // various requests.
    ArrayList<String> words = new ArrayList<String>();
    String returned = "";
    

    public String handleRequest(URI url) {
        if (url.getPath().equals("/")) {
            for(int i=0;i<words.size();i++){
                returned+= "\n"+ words.get(i);
            }
            return String.format(returned);    
        }
        System.out.println("Path: " + url.getPath());
            if (url.getPath().contains("/add-message")) {
                String[] parameters = url.getQuery().split("=");
                    words.add(parameters[1]);
                    returned = "";
                    if(words.size()<2){
                        returned+=words.get(0);
                    }
                    else{
                        for(int i=0;i<words.size();i++){
                            returned+= "\n"+ words.get(i);
                        }
                    }
                    return String.format(returned);
                }
            return "404 Not Found!";
        
    }
}

class StringServer {
    public static void main(String[] args) throws IOException {
        if(args.length == 0){
            System.out.println("Missing port number! Try any number between 1024 to 49151");
            return;
        }

        int port = Integer.parseInt(args[0]);

        Server.start(port, new Handler());
    }
}
```
The server returns these words when arguments are passed through the url:

![Image](Screen%20Shot%202023-01-26%20at%205.49.54%20PM.png)

Here the HandleRequest method is called. This method uses the argument that is passed in through the url of the website. The arguments from the url are taken in by the getPath() method that takes in parts of the URL in as a string. In this case, the argument that was passed in was the string "Hello". The getQuery() method uses the '=' sign as a delimiter to separate the argument from the rest of the URL. The method takes in the value after the equal sign and adds it to the array of words that are to be printed. The value of the array is not the only one that is changed. The value of the string that is printed is also changed to "Hello".

![Image](Screen%20Shot%202023-01-26%20at%205.50.21%20PM.png)

In this second example, the handler method is again called with a new argument: "WhatIsUp". This method again gets this parameter through the url and using the delimiter of the "=" to find this parameter. This parameter is added to the arraylist of the words to change the value of the arraylist to {Hello, WhatIsUp}. The value of the string that is printed is also changed to include "WhatIsUp".

**Part 2: Lab 3 bugs**

This JUnit test case worked as a failure inducing input because instead of reversing the array correctly, the method changed values in the back of the array before they could be applied to the front of the array:
```
@Test 
public void testMultipleReverseInPlace() {
	int[] input1 = { 1, 2, 3, 4 };
	ArrayExamples.reverseInPlace(input1);
	assertArrayEquals(new int[]{ 4, 3, 2, 1 }, input1);
}
``` 

   This JUnit test case did not induce a failure because it has an empty array, so the method will just return an empty array back. In this case, there are no values that are changed, so the method returns the correct value:
```
@Test
public void testEmptyReverseInPlace() {
	int[] input1 = { };
	ArrayExamples.reverseInPlace(input1);
	assertArrayEquals(new int[]{ }, input1);
}
```
This is what the result of the JUnit tests looked like:
![Image](Screen%20Shot%202023-02-11%20at%207.45.53%20AM.png)


The method before it was changed:
```
static void reverseInPlace(int[] arr) {
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = arr[arr.length - i - 1];
    }
  }
```
The method after it was changed:
```
  static void reverseInPlace(int[] arr) {
    int holder;
    for(int i=0;i<arr.length/2;i++){
      holder = arr[arr.length-1-i];
      arr[arr.length-1-i] = arr[i];
      arr[i] = holder;
    }
  }
  ```
  
  This fix addresses the issue by storing the old data in a separate array before it starts reversing the original array. Previously, the reversing method would change values in the array while iterating through it which would lead to the array being reversed incorrectly. This fix allows the program to be able to correctly reverse the array instead of getting an incorrect result.
  
  **Part 3: What I learned**
  
  One of the new things that I learned in lab from these two weeks was how to push code to Github. I also learned how to take code from Github, edit it in Visual Studio Code and push my changes onto Github. I had never previously known that Github code could be so easily accessed and edited from the desktop.
