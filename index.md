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

Which methods in your code are called?
What are the relevant arguments to those methods, and the values of any relevant fields of the class?
How do the values of any relevant fields of the class change from this specific request? If no values got changed, explain why.
**Part 2: Lab 3 bugs**
This JUnit test case worked as a failure inducing input:
```
    @Test 
	public void testMultipleReverseInPlace() {
    int[] input1 = { 1, 2, 3, 4 };
    ArrayExamples.reverseInPlace(input1);
    assertArrayEquals(new int[]{ 4, 3, 2, 1 }, input1);
	}
   ```
   This JUnit test case did not induce a failure:
   ```
  @Test
  public void testEmptyReverseInPlace() {
    int[] input1 = { };
    ArrayExamples.reverseInPlace(input1);
    assertArrayEquals(new int[]{ }, input1);
	}

```
The method before it was changed:
```

```
The method after it was changed:
```
  static void reverseInPlace(int[] arr) {
    int[] oldData  = new int[arr.length];
    for(int i=0;i<oldData.length;i++){
      oldData[i] = arr[i];
    }
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = oldData[arr.length - i - 1];
    }
  }
  ```
