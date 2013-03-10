---
title: Getting Started with SQL CLR Part 2
author: Rob
layout: post
permalink: /archives/getting-started-with-sql-clr-part-2/
dsq_thread_id:
  - 896709328
categories:
  - Databases
  - SQL Server
---
# 

As you start playing with SQL CLR, you learn pretty quick that the built in namespaces can be a little handicapping. You can skip this by creating an 'unsafe' assembly in your database. In the following demo, we're going to load a 3rd party .library for XMPP messaging into our SQL Server instance, and use a stored procedure to send XMPP messages.

The XMPP library I'll be using is [agsXMPP][1]. As you'll see in my code below, I tend to just use a common shared library folder so I can house various libraries and it makes it easier from an organizational standpoint to cram them into the database. I use the following code to 'cram' the library into my instance so I'll be able to use it in a CLR procedure:

 [1]: http://www.ag-software.de/agsxmpp-sdk.html

    1.  &nbsp;
    
    2.  CREATE ASSEMBLY &#91;agsXMPP&#93;
    
    3.  FROM 'C:SubversionShared LibsagsxmppagsXMPP.dll'
    
    4.  WITH PERMISSION_SET = UNSAFE
    
    5.  GO
    
    6.  &nbsp;

Once that is out of the way, we can roll our CLR proc, deploy and message to our hearts content (or until the network drops)

    1.  &nbsp;
    
    2.  using System;
    
    3.  using System.Data;
    
    4.  using System.Data.SqlClient;
    
    5.  using System.Data.SqlTypes;
    
    6.  using Microsoft.SqlServer.Server;
    
    7.  &nbsp;
    
    8.  using agsXMPP;
    
    9.  using agsXMPP.protocol.client;
    
    10. // yes,  domains and names have been changed to protect the innocent servers.
    
    11. public partial class StoredProcedures
    
    12. &#123;
    
    13. 	&#91;Microsoft.SqlServer.Server.SqlProcedure&#93;
    
    14. 	public static void clr_SendXMPP&#40;&#41;
    
    15. 	&#123;
    
    16. 		XmppClientConnection xmpp;
    
    17. 		xmpp = [new][2] XmppClientConnection&#40;&#41;;
    
    18. 		xmpp.AutoPresence = true;
    
    19. &nbsp;
    
    20. 		xmpp.AutoResolveConnectServer = true;
    
    21. 		xmpp.Port = 5222;
    
    22. 		xmpp.UseSSL = false;
    
    23. 		xmpp.Server = "xmpp.datachomp.com";
    
    24. 		xmpp.Username = "SQLAlert";
    
    25. 		xmpp.Password = "SqlClr";
    
    26. &nbsp;
    
    27. 		xmpp.Open&#40;&#41;;
    
    28. &nbsp;
    
    29. 		xmpp.OnLogin  = delegate&#40;object o&#41; &#123; xmpp.Send&#40;[new][2] Message&#40;[new][2] Jid&#40;"Gatir@xmpp.datachomp.com"&#41;, MessageType.chat, "This really should be a variable yes?"&#41;&#41;; &#125;;
    
    30. 	&#125;
    
    31. &#125;;
    
    32. &nbsp;

 [2]: http://www.google.com/search?q=new msdn.microsoft.com