

# JAVASCRIPT

JavaScript runs in the UI thread of the browser and any long running process will lock the UI, making it unresponsive. Additionally, there is an upper limit on the execution time for JavaScript and the browser will ask the user whether to continue the execution or not.

 three available solutions for hanfling async operations in current browsers, and node 7+:
 
 
Promises with async/await (ES2017+, available in older browsers if you use a transpiler or regenerator)
 
Callbacks (popular in node)

Promises with then() (ES2015+, available in older browsers if you use one of the many promise libraries)



## SOURCES

[https://stackoverflow.com/questions/14220321/how-to-return-the-response-from-an-asynchronous-call](https://stackoverflow.com/questions/14220321/how-to-return-the-response-from-an-asynchronous-call)
