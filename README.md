# "Refactoring TypeScript: Keeping your code healthy" Cheat Sheet

To start with, the main purpose of a method is **not** sharing code, it is to help us **abstract** parts of our code and give that code a more understandable label. 

## **⁉Null Checks | Null Object Pattern**

- Arrays or Functions
  - Return Array || Empty collection
  - For nested objects. Set values to Array || Empty Collection
- Objects
  - Use a null object that handles the null check
  - IBoss
    - Boss
    - NullBoss

## **🍧Special Case Pattern**

- ❌Instead of using enums as
  - StatusEnum
    - Pending
    - PaymentRejected
    - Canceled
  - Order
    - Status : StatusEnum
    - PlaceOrder()
      - If(status === enu.status.Pending) {}
      - If(status === enu.status.PaymentRejected) {}
      - If(status === enu.status.Canceled) {}
- ✔Create a Class of each case
  - IOrder
    - PlaceOrder()
  - PendingOrder implements IOrder
  - PaymentRejectedOrder implements IOrder
  - CanceledOrder implements IOrder

## **🚥Conditionals**

- Combine
  - Assign a constant for each conditional
    - const isAdmin:boolean = user.role === &quot;admin&quot; etc...
    - if(isAdmin &amp;&amp; isActive &amp;&amp; canEdit)
  - Combine common conditionals to a single constant
    - if(activeAdminCanEdit)
- Extract to Class
  - Extract conditional logic on User Class
    - User.isActive()
    - User.canEdit()
- Extract to explicit Classes
  - Class UserIsActiveAdmin
- Pipe Classes
  - IPipeCondition
    - Check()
  - Class UserIsActiveAdmin implements IPipeCondition
  - Then create a collection of IPipeCondition(s) and a for loop to check() the conditions

## **🚨Guard Clauses**

- Fail fast
- Sort if clauses by which who will fail first
- Best used with _Gate Classes_

## **🚪Gate Classes**

- For clauses that are considered gates for big code blocks
- Create classes that handle the if clause and the code blocks then throw an exception on failure

## **🛑Primitive Overuse**

- ❌Instead of getting primitive values like
  - Const domain:string = email.replace(/.\*@/,&quot;&quot;);
  - const userName = email.replace(domain, &quot;&quot;);
- ✔Create a class (Value Object) like
  - Email
    - Value
    - Domain = Value.replace(/.\*@/,&quot;&quot;);
    - Username = domain.replace(domain, &quot;&quot;);

## **🧩Deceptive Booleans**

- ❌Instead of filling a User class with functions that will be used once like
  - User.isAuthenticated()
  - User.isActive()
  - User.alreadyHadSession()
  - User.isLockedOut()
  - User.isFirstLogin()
- ✔Change the isAuthenticated function to return an enum
  - AuthenticationResultenum
    - InvalidCredentials
    - UserIsNotActive
    - HasExistingSession
    - IsLockedOut
    - IsFirstLogin
    - Successful

## **✂Lengthy Method Signatures**

- ❌Instead of method getUsers() with params like
  - IncludeInactive:boolean
  - FilterText:string
  - OrderByName:boolean
- ✔Make this method private and create a public method for each use like
  - GetActiveUsers()
    - Return getUsers(false)
  - GetInactiveUsers
    - Return getUsers(true)

## **🥊Lengthy Conditionals > Strategy Pattern**

- Create Enum
  - OrderStatusEnum
    - Pending
    - Shipped
    - Cancelled
- Create strategy
  - Const strategies: Function[] = []
- Push strategies
  - Strategies[OrderStatusEnum.Pending] = processPendingOrder
  - Strategies[OrderStatusEnum.Shipped] = processShippedOrder
  - Strategies[OrderStatusEnum.Cancelled] = processCancelledOrder
- Execute the appropriate strategy
  - Strategies[order.getStatus()]

## 🗒Final Notes

- Must specify return type of a function
- If a function is to be used from different locations
  - Must be placed in a 🌟reachable🌟 Class
- If a function is a service, it should have its own try catch block
- If a method is longer than the screen, then it's probably too long
