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

    1.  &nbsp;
    
    2.  SET @TheID = SCOPE_IDENTITY&#40;&#41;;
    
    3.  RETURN @TheID
    
    4.  &nbsp;

My personal thought on the matter is that you use the Return Values are for determining the execution health of the procedure you called, and that to get an ID value either from a record set or an OUTPUT parameter. This allows a separation of concerns and provides a way for an application to completely understand the intent of the procedure that executed. Lets jump to the code to see what I mean:

    1.  &nbsp;
    
    2.  --drop table dbo.pizza
    
    3.  CREATE TABLE Pizza &#40;pizzaid TINYINT IDENTITY&#40;1,1&#41;, pizzasize VARCHAR&#40;10&#41;, numberoftoppings TINYINT, pizzaname VARCHAR&#40;40&#41; &#41;
    
    4.  &nbsp;
    
    5.  CREATE PROCEDURE dbo.asp_InsertNewPizza
    
    6.  	&#40;@PizzaSize VARCHAR&#40;10&#41;,@NumberofToppings TINYINT,@pizzaName VARCHAR&#40;40&#41;,@PizzaID TINYINT OUTPUT
    
    7.  AS
    
    8.  &nbsp;
    
    9.  BEGIN TRY
    
    10. INSERT INTO dbo.Pizza &#40;pizzaSize, NumberOftoppings,pizzaname&#41;
    
    11. VALUES &#40;@PizzaSize, @NumberofToppings, @pizzaName&#41;
    
    12. &nbsp;
    
    13. SELECT @PizzaID = SCOPE_IDENTITY&#40;&#41;;
    
    14. RETURN ;
    
    15. END Try
    
    16. BEGIN Catch
    
    17. 	RETURN -1
    
    18. END Catch
    
    19. GO
    
    20. &nbsp;
    
    21. DECLARE @InsertedID TINYINT, @RunOK INT;
    
    22. EXEC @RunOK = dbo.asp_InsertNewPizza @pizzaSize = 'Medium',@NumberOfToppings=2, @PizzaName='Medium Cheese and Sausage', @PizzaID=@InsertedID OUT
    
    23. SELECT @RunOK, @InsertedID
    
    24. &nbsp;

So, when we run the above code, we get a 0 for the return value (@RunOK), and a 1 for the inserted ID. Had we received a -1 for the return value, we would know that something was jacked up and not to trust the value of the OUTPUT param. Of course, like many demo's, this is an incredibly simple demo just to show proof of concept. We could go a huge discourse about handling failures inside the procedure, application level transactions, or any number of scenarios that might result in a "Fine, in that case don't do the above" but all that does is take away from the general message of separation of concerns and being able to quickly convey what actually happened. Lets look at this slight alteration to our operation:

    1.  &nbsp;
    
    2.  CREATE PROCEDURE dbo.asp_InsertNewPizza
    
    3.  	&#40;@PizzaSize VARCHAR&#40;10&#41;,@NumberofToppings TINYINT,@pizzaName VARCHAR&#40;40&#41;,@PizzaID TINYINT OUTPUT
    
    4.  AS
    
    5.  &nbsp;
    
    6.  IF EXISTS &#40;SELECT pizzaID FROM dbo.Pizza WHERE pizzaname = @pizzaName&#41;
    
    7.  BEGIN
    
    8.  	SET @PizzaID = NULL;
    
    9.  	RETURN 1;	
    
    10. END
    
    11. ELSE
    
    12. BEGIN TRY
    
    13. 	INSERT INTO dbo.Pizza &#40;pizzaSize, NumberOftoppings,pizzaname&#41;
    
    14. 	VALUES &#40;@PizzaSize, @NumberofToppings, @pizzaName&#41;
    
    15. &nbsp;
    
    16. 	SELECT @PizzaID = SCOPE_IDENTITY&#40;&#41;;
    
    17. 	RETURN ;
    
    18. END TRY
    
    19. BEGIN CATCH
    
    20. 	RETURN -1
    
    21. END CATCH
    
    22. &nbsp;
    
    23. GO
    
    24. &nbsp;

Without catching the Return Values... if we get a NULL for the identity, how do we know if it is from an issue with inserting the values or an issue with a pizza name already existing? If we capture the ReturnValue and see that it is >= 0 we know that it wasn't from an error, but that a certain business rule had been met.

Well this is all good and well to us the database people, but what do we do if the app devs say it is too hard to won't help them. Here is just a basic call in C# with ADO.NET and how it could be used:

    1.  &nbsp;
    
    2.  SqlCommand databaseCMD = [new][1] SqlCommand &#40;"asp_InsertNewPizza", pizzaConn&#41;;
    
    3.  &nbsp;
    
    4.  databaseCMD.CommandType = CommandType.StoredProcedure;
    
    5.  &nbsp;
    
    6.  SqlParameter PizzaSize = databaseCMD.Parameters.Add &#40;"@pizzaSize", SqlDbType.VarChar, 10&#41;;
    
    7.  PizzaSize.Direction = ParameterDirection.Input;
    
    8.  SqlParameter NumberOfToppings = databaseCMD.Parameters.Add &#40;"@NumberOfToppings", SqlDbType.TinyInt&#41;;
    
    9.  NumberOfToppings.Direction = ParameterDirection.Input;
    
    10. SqlParameter PizzaName = databaseCMD.Parameters.Add &#40;"@PizzaName", SqlDbType.VarChar, 40&#41;;
    
    11. PizzaName.Direction = ParameterDirection.Input;
    
    12. &nbsp;
    
    13. &nbsp;
    
    14. SqlParameter PizzaID = databaseCMD.Parameters.Add &#40;"@PizzaID", SqlDbType.TinyInt&#41;;
    
    15. PizzaID.Direction = ParameterDirection.Output ;
    
    16. SqlParameter ReturnValue = databaseCMD.Parameters.Add &#40;"RetVal", SqlDbType.Int&#41;;
    
    17. RetVal.Direction = ParameterDirection.ReturnValue;
    
    18. &nbsp;
    
    19. int pizzaID;
    
    20. pizzaConn.Open&#40;&#41;;
    
    21. &nbsp;
    
    22. pizzaID =databaseCMD.ExecuteScalar &#40;&#41;.ToString&#40;&#41; ;
    
    23. &nbsp;
    
    24. if &#40;ReturnValue == &#41;&#123;
    
    25. ResponseWrite&#40;"The Pizza was added. The pizzaID is : "   pizzaID.ToString&#40;&#41; &#41;;
    
    26. ResponseWreite&#40;"Return Value: "   RetVal.Value&#41;;
    
    27. &#125;
    
    28. else if&#123; ReturnValue == 1&#41; &#123;
    
    29. ResponseWrite&#40;"Sorry friend, that pizza name already exists."&#41;;
    
    30. ResponseWreite&#40;"Return Value: "   RetVal.Value&#41;;
    
    31. &#125;
    
    32. else &#123; FreakOut&#40;&#41;; &#125;
    
    33. &nbsp;

 [1]: http://www.google.com/search?q=new msdn.microsoft.com