# Do a Simple HTTP Request in Java

---

## HttpUrlConnection
   
The HttpUrlConnection class allows us to perform basic HTTP requests without the use of any additional libraries. All the classes that we need are part of the java.net package.


The disadvantages of using this method are that the code can be more cumbersome than other HTTP libraries and that it does not provide more advanced functionalities such as dedicated methods for adding headers or authentication.


## Creating a Request

We can create an HttpUrlConnection instance using the openConnection() method of the URL class. Note that this method only creates a connection object but doesn't establish the connection yet.

The HttpUrlConnection class is used for all types of requests by setting the requestMethod attribute to one of the values: GET, POST, HEAD, OPTIONS, PUT, DELETE, TRACE.

Let's create a connection to a given URL using GET method:

    URL url = new URL("http://example.com");
    HttpURLConnection con = (HttpURLConnection) url.openConnection();
    con.setRequestMethod("GET");


## Adding Request Parameters

If we want to add parameters to a request, we have to set the doOutput property to true, then write a String of the form param1=value¶m2=value to the OutputStream of the HttpUrlConnection instance:

    Map<String, String> parameters = new HashMap<>();
    parameters.put("param1", "val");
    
    con.setDoOutput(true);
    DataOutputStream out = new DataOutputStream(con.getOutputStream());
    out.writeBytes(ParameterStringBuilder.getParamsString(parameters));
    out.flush();
    out.close();

## Configuring Timeouts

HttpUrlConnection class allows setting the connect and read timeouts. These values define the interval of time to wait for the connection to the server to be established or data to be available for reading.

To set the timeout values, we can use the setConnectTimeout() and setReadTimeout() methods


## Handling Cookies

The java.net package contains classes that ease working with cookies such as CookieManager and HttpCookie.

First, to read the cookies from a response, we can retrieve the value of the Set-Cookie header and parse it to a list of HttpCookie objects:

    String cookiesHeader = con.getHeaderField("Set-Cookie");
    List<HttpCookie> cookies = HttpCookie.parse(cookiesHeader);
    Next, we will add the cookies to the cookie store:
    
    cookies.forEach(cookie -> cookieManager.getCookieStore().add(null, cookie));


## Handling Redirects

We can enable or disable automatically following redirects for a specific connection by using the setInstanceFollowRedirects() method with true or false parameter:

    con.setInstanceFollowRedirects(false);

It is also possible to enable or disable automatic redirect for all connections:

    HttpUrlConnection.setFollowRedirects(false);


## Reading the Response

Reading the response of the request can be done by parsing the InputStream of the HttpUrlConnection instance.

To execute the request, we can use the getResponseCode(), connect(), getInputStream() or getOutputStream() methods:

    int status = con.getResponseCode();

## Reading the Response on Failed Requests

If the request fails, trying to read the InputStream of the HttpUrlConnection instance won't work. Instead, we can consume the stream provided by HttpUrlConnection.getErrorStream().


## Building the Full Response

It's not possible to get the full response representation using the HttpUrlConnection instance.

However, we can build it using some of the methods that the HttpUrlConnection instance offers:

    public class FullResponseBuilder {
    public static String getFullResponse(HttpURLConnection con) throws IOException {
    StringBuilder fullResponseBuilder = new StringBuilder();
    
            // read status and message
    
            // read headers
    
            // read response content
    
            return fullResponseBuilder.toString();
        }
    }