# Error-handlers

This is a simple error handling package who's functions can be used throughout a node application.

# Documentation
First:
<pre> npm i un-handle </pre>
## Error handlers:
### 1) ErrorResponse
The ErrorResponse function takes three parameters, the response, error, and http status code.
<code>ErrorResponse(response, error, http_status_code)</code>
### 2) SuccessResponse
The SuccessResponse function takes the same parameters as above but since it returns a response object, instead of an error.
<code>SuccessResponse(response, response_data, http-status-code)</code>
### 3) CatchError
The CatchError function takes a promise as the parameter, and handles the promise effectivly with the <code>.catch()</code> function.
<code>CatchError(promise)</code>
### 4) ThrowError
It does just that - but also has an extra parameter <code>log: boolean</code> which will log the error in the server.
<code>ThrowError(error_message, log)</code>

## Example usage:
To use the error hanlders throughout your application: 
<code> const unHandle = require('un-handle')</code>
You can either require this module wherever necessary, or in the express app.js you can add something like this:
<pre>
const handle = require('error-handlers');
// Global functions for handling errors
global.pe = handle.ParseError;
global.to = handle.CatchError;
global.TE = handle.ThrowError;
global.ReE = handle.ErrorResponse;
global.ReS = handle.SuccessResponse;
</pre>
You could also make a separate file containing all (if any) global functions and declare add the code there instead.
Below is an example of these functions in use.
In this example, the author is using the sequelize ORM.
<pre>
const update = async function(req, res){
    let err, user, data, client, able;
    user = req.user;
    client = req.client;
    data = req.body;
    user.set(data);
    
    [err, user] = await to(user.save());
    if(err) return ReE(res, err, 422);
    return ReS(res, {message :'Updated User: '+ client.clientId});
}
</pre>

