# Trigger handler pattern in Salesforce

The Trigger Handler pattern is a best practice for managing Apex triggers in the Salesforce platform. This pattern helps ensure the trigger code is well-organized, efficient, and maintainable.

## 1. One Trigger Per Object
```java 
trigger AccountTrigger on Account( after insert, after update, before insert, before update) {
    AccountTriggerHandler handler = new AccountTriggerHandler(Trigger.isExecuting, Trigger.size);
    
    if( Trigger.isInsert ){
        if(Trigger.isBefore) {
            handler.OnBeforeInsert(trigger.New);
        }
        else {
            handler.OnAfterInsert(trigger.New);
        }
    }
    else if ( Trigger.isUpdate ) {
        if(Trigger.isBefore){
            handler.OnBeforeUpdate(trigger.New ,trigger.Old,Trigger.NewMap,Trigger.OldMap);
        }
        else{
            handler.OnAfterUpdate(trigger.New ,trigger.Old,Trigger.NewMap,Trigger.OldMap);
        }
    }
}
```
## 2. Trigger Handler Class
```java 
public with sharing class AccountTriggerHandler 
{
    private boolean m_isExecuting = false;
    private integer BatchSize = 0;
    public static boolean IsFromBachJob ;
    public static boolean isFromUploadAPI=false;
    
    public AccountTriggerHandler(boolean isExecuting, integer size)
    {
        m_isExecuting = isExecuting;
        BatchSize = size;
    }
            

    public void OnBeforeInsert(List<Account> newAccount)
    {
        system.debug('Account Trigger On Before Insert');
    }
    public void OnAfterInsert(List<Account> newAccount)
    {
        system.debug('Account Trigger On After Insert');
    }
    public void OnAfterUpdate( List<Account> newAccount, List<Account> oldAccount, Map<ID, Account> newAccountMap , Map<ID, Account> oldAccountMap )
    {
        system.debug('Account Trigger On After Update ');
        AccountActions.updateContact (newAccount);
    }
    public void OnBeforeUpdate( List<Account> newAccount, List<Account> oldAccount, Map<ID, Account> newAccountMap , Map<ID, Account> oldAccountMap )
    {
        system.debug('Account Trigger On Before Update ');
    }

    @future 
    public static void OnAfterUpdateAsync(Set<ID> newAccountIDs)
    {

    }      
    public boolean IsTriggerContext
    {
        get{ return m_isExecuting;}
    }
    
    public boolean IsVisualforcePageContext
    {
        get{ return !IsTriggerContext;}
    }
    
    public boolean IsWebServiceContext
    {
        get{ return !IsTriggerContext;}
    }
    
    public boolean IsExecuteAnonymousContext
    {
        get{ return !IsTriggerContext;}
    }
} 
```
## 3. Trigger Helper Class
```java 
public with sharing class AccountActions 
{
    public static void updateContact ( List<Account> newAccount)
    {
        // Add your logic here
    }
}
```
![screenshot](https://github.com/NguyenXuanThin/Trigger-pattern/blob/main/image.png)
## Advantage of Trigger Handler pattern

* Apex Class that handles trigger logic
* Allows code to be called from other code or tests
* Uses specific Trigger contexts and Trigger variables for routing
* Keep Triggers Simple
* Allow for greater flexibility
* Make code reusable
* Unit tests are much easier

## Here are a few best practices for triggers in Salesforce:

* **Bulkify your Code:** Always write triggers that can handle multiple records at a time to respect Salesforce’s governor limits.
* **Avoid Recursive Triggers:** Ensure your trigger doesn’t re-fire itself, leading to an infinite loop.
* **One Trigger per Object:** To keep better control of the order of operations, it’s recommended to have only one trigger per object.
* **Keep Business Logic in Helper Classes:** Instead of writing all the business logic inside the trigger, write the logic in a separate class and call this class from the trigger.
* **Test Coverage:** Always aim for a minimum of 75% test coverage to ensure the trigger works as expected.
* **Error Handling:** Include proper error handling to display meaningful error messages to the end user.

## There are several limitations of triggers in Salesforce:

* **Governor Limits:** Salesforce enforces various limits on the number of database operations, the amount of CPU time, and the memory capacity that triggers can use.
* **Order of Execution:** The order in which triggers are executed can’t be explicitly controlled.
* **Testing and Deployment:** Triggers must have at least 75% test coverage in order to be deployed to a production environment.
* **Recursive Triggers:** Avoiding recursive triggers can be complex. If the same trigger is fired continuously, you may run into infinite loops.
* **Complexity:** Triggers can become complex, especially as more business logic is added. This can lead to difficult-to-maintain code.
## Note:
  This pattern can apply interface to implement trigger method.
