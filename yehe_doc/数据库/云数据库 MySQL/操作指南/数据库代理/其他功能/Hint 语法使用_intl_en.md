
This document describes how to use the hint syntax on the database proxy.

The hint syntax can be used to forcibly execute SQL requests on the specified instance. A hint has the highest routing priority. For example, it is not subject to consistency and transaction constraints. Please carefully evaluate whether it is required in your business scenario before using it.

>!When using the MySQL command line tool to connect and use the `HINT` statement, you need to add the `-c` option in the command; otherwise, the hint will be filtered by the tool.
>
Currently, three types of hints are supported:
- Assign to the source instance for execution:
```
/* to master */
/*FORCE_MASTER*/   
```  
- Assign to a read-only instance for execution:
```
/* to slave */
/*FORCE_SLAVE*/  
```  
- Specify an instance for execution:
```
/* to server server_name*/
```
`server_name` can be a short ID, such as `/* to server test_ro_1 */`.

