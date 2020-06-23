# Batch Apex
This is a snippet for making a batch class.

> Read more on Batch Apex on [Admin2Dev](https://admin2dev.com/apex-starter/14.html)

## Batch Class
```
global class ClassName implements Database.Batchable<sObject> {
    global (Database.QueryLocator | Iterable<sObject>) start(Database.BatchableContext bc) { //Choose between Database.QueryLocator or List<sObject>
    	//Query
      return 'SELECT FROM WHERE LIMIT';
    }
    global void execute(Database.BatchableContext bc, List<sObject> iteratorList){
    	//Process the query
    }
    global void finish(Database.BatchableContext bc){
	    //Post Processing
    }
}
```

## Test Class
```
@isTest
public class ClassNameTest {
    @testSetup static void TestClass(){
    //Insert operations
    }

    static testmethod void testMethodName() {

        Test.startTest();
        ClassName batchTester = new ClassName();
        ID BatchID = Database.executeBatch(batchTester);
        Test.stopTest();

        System.assertEquals(expected, actual, 'message');
    }

}
```
