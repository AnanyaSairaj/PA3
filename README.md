# PA3
# HTTP
## INTRODUCTION
GOAL :To build a simple web proxy server that is capable of relaying HTTP requests from clients to the HTTP servers. 

This proxy server can service multiple clients simultaneously.The client requests have been implemented through the web browser and the telnet. This web server implementation is tested with Firefox web browser. It also implements basic caching,cache timeout and link prefetching as per the requirements.

### FILE STRUCTURE IMPLEMENTED
```
./README.txt
./proxy_server.c
./Makefile
./IPCache.txt
./blocked.txt
./pagecache.txt
./cache/


```

## IMPLEMENTATION

The server implements only HTTP GET requests with only HTTP/1.0 and HTTP/1.1 as valid versions.For other cases it shows an error.   
* proxy_server.c : It receives the client requests in the main function and passes it on to a hancleClientrequest function. This checks if the page already exists in the cache and fetches from the cache if it does or established a new server connection with a remote proxy server to fetch the requested page if it does not.
* IPcache.txt : This text file is used to implement the DNS cacheing. After getting the path the program checks if it is already present in this file.If it is present it parses the file and gets the displays the IP from the file text .Else it queries the DNS server to resolve through gethostbyname().
* Blocked.txt : If a path is present in the file then the error 404 FORBIDDEN is displayed.
* pagecache.txt : Used to check if the page is already present in the cache and if it is ,the timeout value for that page.It refetchs the page from the remote server if the url is not present in the file. It also refetchs the page from the remote server if the timeout value that is entered by the user has exceeded. If the page is present and has not timed out it reads the page from the cache folder and sends the page data.
* cache/ : This folder has all the files that have been requested previously and cached as well as prefetched. When a file is fetched from the remote server and written into the cache the prefetch function parses it for further links and if it finds a fetchable link it forks to create a new simultaneous child process and requests for the that page to the remote server and stores it also in the cache. Another important fact to note is that all the files in the cache folder are named after the md5# values for their specific url .

## IMPLEMENTING CACHING AND PREFETCHING
 
 As mentioned above the caching function is implemented by writing the data received from the remote serrver to the clients request into a file and simultaneously sending it to the client so that when the same page is requested by the client again ,the proxy can just read the contents from it's local cache. This can be done only if the file in the local cache has not exceeded it's specified timeout value.
 
 To implement prefetching the file written into the cache folder has been parsed for <a href tags and the following http:// links have been taken as the url to put together the new GET request to the remote server.This is done in a seperate child process in the background to not hinder the client requests in the foreground. 

## SERVER EXECUTION 

### TO BUILD

```
*  Go to the project folder, cd [FolderName]/
* Run make 
*./server [PORT NUMBER][timeout for cache]]


```

 ## TESTING
 
 ### TO TEST USING THE BROWSER:
 
 ```
example: http://netsys.cs.colorado.edu 
 
 ```
 
### TO TEST USING TELNET

 ```
 telnet 127.0.0.1 <Port number>
 GET <url> <http version>
