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

<code>
	CREATE ASSEMBLY [agsXMPP]
	FROM 'C:SubversionShared LibsagsxmppagsXMPP.dll'
	WITH PERMISSION_SET = UNSAFE
	GO
</code>

Once that is out of the way, we can roll our CLR proc, deploy and message to our hearts content (or until the network drops)
<code>
	using System;
	using System.Data;
	using System.Data.SqlClient;
	using System.Data.SqlTypes;
	using Microsoft.SqlServer.Server;

	using agsXMPP;
	using agsXMPP.protocol.client;
	// yes,  domains and names have been changed to protect the innocent servers.
	public partial class StoredProcedures
	{
		[Microsoft.SqlServer.Server.SqlProcedure]
		public static void clr_SendXMPP()
		{
			XmppClientConnection xmpp;
			xmpp = new XmppClientConnection();
			xmpp.AutoPresence = true;

			xmpp.AutoResolveConnectServer = true;
			xmpp.Port = 5222;
			xmpp.UseSSL = false;
			xmpp.Server = "xmpp.datachomp.com";
			xmpp.Username = "SQLAlert";
			xmpp.Password = "SqlClr";

			xmpp.Open();

			xmpp.OnLogin += delegate(object o) { xmpp.Send(new Message(new Jid("Gatir@xmpp.datachomp.com"), MessageType.chat, "This really should be a variable yes?")); };
		}
	};
</code>

 [2]: http://www.google.com/search?q=new msdn.microsoft.com