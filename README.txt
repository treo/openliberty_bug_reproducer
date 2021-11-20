This reproduces the "server.env" ignoring behavior in runnable OpenLiberty jars.

To reproduce it yourself:
1. Clone this repository
2. Run `mvn clean package` to create the runnable jars
3. Run with java -jar `target/reproducer.jar`

In the message log file (`wlp/usr/servers/reproducer/logs/messages.log`) you will find the folllowing:
```
com.ibm.ws.ssl.config.WSKeyStore                             I CWPKI0819I: The default keystore is not created because a password is not configured on the <keyStore id="defaultKeyStore"/> element, and the 'keystore_password' environment variable is not set. 
```

This is despite the `keystore_password` environment varible being set in the server.env file. 

This will result in all sorts of SSL related things being broken. 

There are multiple possible workarounds:
1. Define your own keystore 
2. Specify JVM options in the jvm.options file. 

The branch `jvm_options` shows that the jvm options workarounds works.

Instead of the previously shown error message, you will now see the following:

```
com.ibm.ws.ssl.config.WSKeyStore                             A CWPKI0820A: The default keystore has been created using the 'keystore_password' environment variable.
```