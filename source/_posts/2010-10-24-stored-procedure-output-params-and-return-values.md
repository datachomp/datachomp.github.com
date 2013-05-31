---
title: Stored Procedure Output Params and Return Values
author: Rob
layout: post
permalink: /archives/stored-procedure-output-params-and-return-values/
dsq_thread_id:
  - 889361248
categories:
  - Databases
  - SQL Server
---
# 

Many times I see abuse of the Return Value in a stored procedure. What I mean by this is essentially using the Return Value to send back say an identity value or something like:
<code>
	SET @TheID = SCOPE_IDENTITY();
	RETURN @TheID
</code>

My personal thought on the matter is that you use the Return Values are for determining the execution health of the procedure you called, and that to get an ID value either from a record set or an OUTPUT parameter. This allows a separation of concerns and provides a way for an application to completely understand the intent of the procedure that executed. Lets jump to the code to see what I mean:
<code>
	--drop table dbo.pizza
	CREATE TABLE Pizza (pizzaid TINYINT IDENTITY(1,1), pizzasize VARCHAR(10), numberoftoppings TINYINT, pizzaname VARCHAR(40) )

	CREATE PROCEDURE dbo.asp_InsertNewPizza
		(@PizzaSize VARCHAR(10),@NumberofToppings TINYINT,@pizzaName VARCHAR(40),@PizzaID TINYINT OUTPUT
	AS

	BEGIN TRY
	INSERT INTO dbo.Pizza (pizzaSize, NumberOftoppings,pizzaname)
	VALUES (@PizzaSize, @NumberofToppings, @pizzaName)

	SELECT @PizzaID = SCOPE_IDENTITY();
	RETURN 0;
	END Try
	BEGIN Catch
		RETURN -1
	END Catch
	GO

	DECLARE @InsertedID TINYINT, @RunOK INT;
	EXEC @RunOK = dbo.asp_InsertNewPizza @pizzaSize = 'Medium',@NumberOfToppings=2, @PizzaName='Medium Cheese and Sausage', @PizzaID=@InsertedID OUT
	SELECT @RunOK, @InsertedID
</code>

So, when we run the above code, we get a 0 for the return value (@RunOK), and a 1 for the inserted ID. Had we received a -1 for the return value, we would know that something was jacked up and not to trust the value of the OUTPUT param. Of course, like many demo's, this is an incredibly simple demo just to show proof of concept. We could go a huge discourse about handling failures inside the procedure, application level transactions, or any number of scenarios that might result in a "Fine, in that case don't do the above" but all that does is take away from the general message of separation of concerns and being able to quickly convey what actually happened. Lets look at this slight alteration to our operation:
<code>
	CREATE PROCEDURE dbo.asp_InsertNewPizza
		(@PizzaSize VARCHAR(10),@NumberofToppings TINYINT,@pizzaName VARCHAR(40),@PizzaID TINYINT OUTPUT
	AS

	IF EXISTS (SELECT pizzaID FROM dbo.Pizza WHERE pizzaname = @pizzaName)
	BEGIN
		SET @PizzaID = NULL;
		RETURN 1;	
	END
	ELSE
	BEGIN TRY
		INSERT INTO dbo.Pizza (pizzaSize, NumberOftoppings,pizzaname)
		VALUES (@PizzaSize, @NumberofToppings, @pizzaName)

		SELECT @PizzaID = SCOPE_IDENTITY();
		RETURN 0;
	END TRY
	BEGIN CATCH
		RETURN -1
	END CATCH

	GO
</code>

Without catching the Return Values... if we get a NULL for the identity, how do we know if it is from an issue with inserting the values or an issue with a pizza name already existing? If we capture the ReturnValue and see that it is >= 0 we know that it wasn't from an error, but that a certain business rule had been met.

Well this is all good and well to us the database people, but what do we do if the app devs say it is too hard to won't help them. Here is just a basic call in C# with ADO.NET and how it could be used:
<code>
	SqlCommand databaseCMD = new SqlCommand ("asp_InsertNewPizza", pizzaConn);

	databaseCMD.CommandType = CommandType.StoredProcedure;

	SqlParameter PizzaSize = databaseCMD.Parameters.Add ("@pizzaSize", SqlDbType.VarChar, 10);
	PizzaSize.Direction = ParameterDirection.Input;
	SqlParameter NumberOfToppings = databaseCMD.Parameters.Add ("@NumberOfToppings", SqlDbType.TinyInt);
	NumberOfToppings.Direction = ParameterDirection.Input;
	SqlParameter PizzaName = databaseCMD.Parameters.Add ("@PizzaName", SqlDbType.VarChar, 40);
	PizzaName.Direction = ParameterDirection.Input;


	SqlParameter PizzaID = databaseCMD.Parameters.Add ("@PizzaID", SqlDbType.TinyInt);
	PizzaID.Direction = ParameterDirection.Output ;
	SqlParameter ReturnValue = databaseCMD.Parameters.Add ("RetVal", SqlDbType.Int);
	RetVal.Direction = ParameterDirection.ReturnValue;

	int pizzaID;
	pizzaConn.Open();

	pizzaID =databaseCMD.ExecuteScalar ().ToString() ;

	if (ReturnValue == 0){
	ResponseWrite("The Pizza was added. The pizzaID is : " + pizzaID.ToString() );
	ResponseWreite("Return Value: " + RetVal.Value);
	}
	else if{ ReturnValue == 1) {
	ResponseWrite("Sorry friend, that pizza name already exists.");
	ResponseWreite("Return Value: " + RetVal.Value);
	}
	else { FreakOut(); }
</code>

 [1]: http://www.google.com/search?q=new msdn.microsoft.com