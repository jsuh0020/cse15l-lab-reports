# Lab Report 2 - Servers and SSH Keys

## Part 1

Below is the code that I made to implement a chat server

### `ChatServer.java`

```java
import java.io.IOException;
import java.net.URI;
import java.util.ArrayList;

class Handler implements URLHandler {
    ArrayList<String> userList = new ArrayList<>();
    ArrayList<String> messageList = new ArrayList<>();
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

### `Server.java` **(Same as Server.java used for SearchEngine Project)**

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
![Image](/lab2_images/lab2_1.png)
