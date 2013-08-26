## Health Checks 

As part of monitoring The Frontier Group's infrastructure, individual applications are monitored for availability using regular HTTP requests to ensure the applications are responding on their home pages. 

In the event of an application component failing, such as a payment or email gateway, additional checks are needed to capture that failure event. These checks are currently named "health checks" and are monitored with a Nagios plugin that parses JSON responses emitted by applications.

### Requests

By default applications will be checked by an unauthenticated GET request to: 

    http://domain.com/health

If a developer wants to use a different URL, they must provide it to a member of the DevOps team.  

### Response Structure

The JSON response structure is as follows.

    ```json
    { :status => '[STATUS]', :message => '[MESSAGE]' }
    ```

Valid values for the status field are: 

|Value|Meaning|
|-----|-------|
|OK|The application is working correctly|
|WARNING|The application is only partially working, or is running in a degraded state|
|CRITICAL|The application is unable to work correctly|

These values are case-insensitive i.e. 'OK', 'Ok' and 'ok' are equivalent.

The message should be relevant to the status field and will be reported in Nagios. 

Any responses that do not match this structure will be treated as critical errors.

### Example Responses

    ```json
    { :status => 'OK', :message => 'All components operational' }
    ```

    ```json
    { :status => 'WARNING', :message => 'Uploads folder not writable, 2 components operational' }
    ```

    ```json
    { :status => 'CRITICAL', :message => 'Payment gateway down' }
    ```
