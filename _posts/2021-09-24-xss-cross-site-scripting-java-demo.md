---
layout: post
title:  "XSS - Cross site scripting Java Demo"
author: Gaurav
categories: [ Java, Security ]
image: assets/images/xss-cross-site-scripting-java-demo/cross-site-scripting-examples.svg
---
Ever wondered how hackers get access to someone's account on any website? 
Or what really happens when you click on an email link received from an unknown address which your organisation 
requests you not to click and report their cyber security team as part of mandatory security training. 
The reason is to prevent you from becoming a victim of cross site scripting practice used by hackers. 
Let's create real applications and observe the practice using java programming platform.

![example 1](/assets/images/xss-cross-site-scripting-java-demo/cross-site-scripting-example1.png)

### Demo
[![Demo](/assets/images/xss-cross-site-scripting-java-demo/xss-cross-site-demo.png)](https://youtu.be/VBSmeyDQRfM)

The demo will show how a customer of a banking site becomes a victim of different cross site scripting practices. We will consider following parties involved:

Banking Website
Attacker Website
Victim's Browser

### Create HTTP Servers in Java
  
Server for Banking website:

<details>
  <summary>Click to expand code!</summary>
  <pre>
  
        package com.banking.application;
        
        import com.sun.net.httpserver.HttpExchange;
        import com.sun.net.httpserver.HttpHandler;
        
        import java.io.IOException;
        import java.io.OutputStream;
        import java.net.InetSocketAddress;
        import java.util.concurrent.Executors;
        import java.util.concurrent.ThreadPoolExecutor;
        
        public class BankHttpServer {
            public static void main(String[] args) throws IOException {
                System.out.println("Starting banking website server");
                com.sun.net.httpserver.HttpServer server = com.sun.net.httpserver.HttpServer.create(new InetSocketAddress("localhost", 8001), 0);
                ThreadPoolExecutor threadPoolExecutor = (ThreadPoolExecutor) Executors.newFixedThreadPool(10);
        
                server.createContext("/banking", new BankHttpServer().new MyHttpHandler());
        
                server.setExecutor(threadPoolExecutor);
        
                server.start();
        
                System.out.println(" Server started on port 8001");
            }
        
            public class MyHttpHandler implements HttpHandler {
                @Override
                public void handle(HttpExchange exchange) throws IOException {
                    if ("GET".equals(exchange.getRequestMethod()) && exchange.getRequestURI().getQuery() != null && exchange.getRequestURI().getQuery().contains("productName")) {
                        handleResponse(exchange, getSearchPage(exchange.getRequestURI().getQuery()));
                    } else {
                        handleResponse(exchange, getDefaultPage());
                    }
                }
        
                private String getDefaultPage() {
                    StringBuilder htmlBuilder = new StringBuilder();
                    htmlBuilder.append("<html><body><form action=\"\" method=\"get\">\n" +
                        " <div>\n" +
                        " <label for=\"name\">Product Name: </label>\n" +
                        " <input size=\"100\" type=\"text\" name=\"productName\" id=\"productName\">\n" +
                        " <input type=\"submit\" value=\"Search\">\n" +
                        " </div>\n" +
                        "</form>").append("</body></html>");
                    return htmlBuilder.toString();
                }
        
                private String getSearchPage(String query) {
                    System.out.println("query: " + query);
                    query = query.substring(query.indexOf("=") + 1);
                    String searchParam = query.replace("+", " ");
                    StringBuilder htmlBuilder = new StringBuilder();
                    htmlBuilder.append("<html><body><form action=\"search\" method=\"get\">\n" +
                        " <div><span>").append(searchParam).append("</span></div></body></html>");
                    return htmlBuilder.toString();
                }
        
                private void handleResponse(HttpExchange exchange, String htmlResponse) throws IOException {
                    OutputStream outputStream = exchange.getResponseBody();
                    exchange.sendResponseHeaders(200, htmlResponse.length());
                    outputStream.write(htmlResponse.getBytes());
                    outputStream.flush();
                    outputStream.close();
                }
            }
        }
  </pre>
</details>

The above code creates an http server running on port 8001. It serves a very basic webpage at http://localhost:8001/banking as shown below:

![Banking 1](/assets/images/xss-cross-site-scripting-java-demo/banking-1.png)

The issue with the website implementation is the use of free input text box where a user is allowed to type anything. Let's do an example search by typing a banking product name.

![Banking](/assets/images/xss-cross-site-scripting-java-demo/banking.gif)

As we can see the response of the website is the name of product and any hacker would see this and try something crazy like this:

![Banking 2](/assets/images/xss-cross-site-scripting-java-demo/banking2.gif)

Since the website was returning the text as is without encoding, it was treated as a legitimate script code by the browser and it executed the code which displayed the alert message. This is a very powerful hacking clue which can be leveraged to read out confidential cookies from the local browser session and sent back to the hacker. Let's create another server application (Attacker's website) as follows:

<details>
    <summary>Expand to see the code!</summary>
    
    <pre>
        
        package com.banking.application;

        import com.sun.net.httpserver.HttpExchange;
        import com.sun.net.httpserver.HttpHandler;
        
        import java.io.IOException;
        import java.io.OutputStream;
        import java.net.InetSocketAddress;
        import java.util.concurrent.Executors;
        import java.util.concurrent.ThreadPoolExecutor;
        
        public class AttackerHttpServer {
        
            public static void main(String[] args) throws IOException {
                System.out.println("Starting attacker website server");
                com.sun.net.httpserver.HttpServer server = com.sun.net.httpserver.HttpServer.create(new InetSocketAddress("localhost", 8002), 0);
                ThreadPoolExecutor threadPoolExecutor = (ThreadPoolExecutor) Executors.newFixedThreadPool(10);
        
                server.createContext("/attacker", new AttackerHttpServer().new MyHttpHandler());
        
                server.setExecutor(threadPoolExecutor);
        
                server.start();
        
                System.out.println(" Server started on port 8002");
            }
        
            public class MyHttpHandler implements HttpHandler {
        
                @Override
                public void handle(HttpExchange exchange) throws IOException {
                    handleResponse(exchange, getDefaultPage());
                }
        
                private String getDefaultPage() {
                    StringBuilder htmlBuilder = new StringBuilder();
                    htmlBuilder.append("alert('Hacked');window.location.href = \"http://localhost:8001/banking\";");
                    return htmlBuilder.toString();
                }
        
                private void handleResponse(HttpExchange exchange, String htmlResponse) throws IOException {
                    OutputStream outputStream = exchange.getResponseBody();
                    exchange.getResponseHeaders().add("Content-Type", "application/javascript");
                    exchange.sendResponseHeaders(200, htmlResponse.length());
                    outputStream.write(htmlResponse.getBytes());
                    outputStream.flush();
                    outputStream.close();
                }
            }
        }
        
    </pre>
    
</details>

The important code snippet from the above code is the getDefaultPage() method which is showing an alert and then redirecting to the banking site:

<pre>

htmlBuilder.append("alert('Hacked');window.location.href = \"http://localhost:8001/banking\";");

</pre>

Now execution of the above code from a webpage served by the banking site can be done as follows:

![Banking 3](/assets/images/xss-cross-site-scripting-java-demo/banking3-2.gif)

Notice that the url which was sent to the 'banking' website as a get request has the script tag embedded.

![Screen shot 1](/assets/images/xss-cross-site-scripting-java-demo/Screen-Shot-1.png)

Now I create an email with the link as follows and send it to 'John' with a very tempting message pushing him to click on the link:

![Email](/assets/images/xss-cross-site-scripting-java-demo/email.png)

Bingo!!!, attacker is able to execute code from his website on victim's browser with the help of the banking website.

### Solution

How can banking website ensure that it doesn't allow such attack? The solution is to use the practice of HTML sanitisation.
There are open source tools and projects that can be used to sanitise an HTML document by removing threatening tags like 
```javascript
<script>, <link>, <object>, <embed>
```
before sending it back as a response. 
OWASP is an online community that produces freely-available articles, methodologies, documentation, tools, and technologies in the field of web application security. 
A very useful project is 'OWASP Java HTML Sanitizer' which provides implementation and also allows custom policies to be configured.

![Screenshot 2](/assets/images/xss-cross-site-scripting-java-demo/Screen-Shot-2.png)

### Conclusion

"nothing is free in this world, just remember everything comes with a price", so don't click the links from unknown senders.