# Lab Report 2 - Servers and SSH Keys

## Part 1

Below is the code that I made to implement a chat server

### `ChatServer.java`

```java
import java.io.IOException;
import java.net.URI;

class Handler implements URLHandler {
    String listOfMessages = "";

    public String handleRequest(URI url) {
        if (url.getPath().equals("/")) {
            return listOfMessages;
        }
        if (url.getPath().equals("/add-message")) {
            String[] parameters = url.getQuery().split("&");
            String[] messages = parameters[0].split("=");
            String[] usernames = parameters[1].split("=");
            if (usernames.length == 1) {
                return "No User Provided";
            }
            if (messages.length == 1) {
                return "No Message Provided";
            }
            if (messages[0].equals("s") && usernames[0].equals("user")) {
                listOfMessages += usernames[1] + ": " + messages[1] + "\n";
            }
            return listOfMessages;
        }
        return "404 Not Found!";
    }
}

class ChatServer {
    public static void main(String[] args) throws IOException {
        if (args.length == 0) {
            System.out.println("Missing port number! Try any number between 1024 to 49151");
            return;
        }

        int port = Integer.parseInt(args[0]);

        Server.start(port, new Handler());
    }
}
```

### `Server.java` **(Same as `Server.java` used for SearchEngine Project)**

```java
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
        } catch (Exception e) {
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

        // create request entrypoint
        server.createContext("/", new ServerHttpHandler(handler));

        // start the server
        server.start();
        System.out.println("Server Started!");
    }
}
```

### Usage of `add-message`

![Image](/lab2_images/lab2_1.png)

* In this first example, the method that is being called when using the `add-message` path is `handleRequest` in the `Handler` class, which is called from the `Server` class.
* This method takes in `url` as its argument, which in this case was `"http://localhost:2883/add-message?s=This is my first example for lab 2&user=Justin"`. The only notable value is the field `listOfMessages` of type `String` which was initially `""`.
* After the request, the `listOfMessages` field is updated to `"Justin: This is my first example for lab 2\n"`.

![Image](/lab2_images/lab2_2.png)

* This second example is similar to the first example, in that the `Server` class is calling the `handleRequest` method in the `Handler` class.
* This method still takes in `url` as its argument, which was `"http://localhost:2883/add-message?s=My Second Example!&user=Suh"`. The `listOfMessages` field is set to `"Justin: This is my first example for lab 2\n"` from the first request.
* After the request, the `listOfMessages` field is updated to `"Justin: This is my first example for lab 2\nSuh:My Second Example!\n"`.

## Part 2

### Absolute Path of Private Key

![Image](/lab2_images/lab2_3.png)

### Absolute Path of Public Key

![Image](/lab2_images/lab2_4.png)

### Accessing `ieng6` Without a Password

![Image](/lab2_images/lab2_5.png)

## Part 3

From today's lab, I learned about commands like `scp`, `mkdir`, `ssh-keygen` which allowed me to login to the `ieng6` server without using a password. I have never seen or used these commands before, and it seems like for today's purposes, they were all related to setting up SSH keys. I also learned that the keys we utilized for easy `ieng6` server access are split into public and private keys, rather than one general key.
