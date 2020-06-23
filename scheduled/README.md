# Scheduled Apex
This is a snippet for making a scheduled class and test class.

> Read more on Scheduled Apex on [Admin2Dev](https://admin2dev.com/apex-starter/20.html)

## Scheduled Class

```
global class ClassName implements Schedulable {
    global void execute(SchedulableContext ctx) {
      // code here
    }
}
```

## Test Class

```
@isTest
public class ClassNameTest {
    public static String CRON_EXP = '0 0 0 15 3 ? 2022'; //define when to run | Seconds Minutes Hours Day_of_month Month Day_of_week optional_year | Midnight, 15 March

    static testmethod void testerMethod(){

        Test.startTest();
        String jobId = System.schedule('ScheduledApexTest', CRON_EXP, new SomeClass()); //create the scheduler instance
        Test.stopTest(); //Scheduler will run immediately after stopTest()

        System.assertEquals(expected, actual, 'message');
    }
}

```

## Creating Scheduled String

### Structure Breakdown

|Name|Values|SpecialCharacters|
|--- |--- |--- |
|Seconds|0–59|None|
|Minutes|0–59|None|
|Hours|0–23|None|
|Day_of_month|1–31|, - * ? / L W|
|Month|1–12 or First three letters of months (JAN - DEC)|, - * /|
|Day_of_week|1–7 or First three letters of the day (SUN - SAT)|, - * ? / L #|
|optional_year|null or 1970–2099|, - * /|

### Special Characters Breakdown
|Special Character|Description|
|--- |--- |
|,|Delimits values. For example, use JAN, MAR, APR to specify more than one month.|
|-|Specifies a range. For example, use JAN-MAR to specify more than one month.|
|*|Specifies all values. For example, if Month is specified as *, the job is scheduled for every month.|
|?|Specifies no specific value. This is only available for Day_of_monthand Day_of_week, and is generally used when specifying a value for one and not the other.|
|/|Specifies increments. The number before the slash specifies when the intervals will begin, and the number after the slash is the interval amount. For example, if you specify 1/5 for Day_of_month, the Apex class runs every fifth day of the month, starting on the first of the month.|
|L|Specifies the end of a range (last). This is only available for Day_of_month and Day_of_week. When used with Day of month, L always means the last day of the month, such as January 31, February 29 for leap years, and so on. When used with Day_of_week by itself, it always means 7 or SAT. When used with a Day_of_week value, it means the last of that type of day in the month. For example, if you specify 2L, you are specifying the last Monday of the month. Do not use a range of values with L as the results might be unexpected.|
|W|Specifies the nearest weekday (Monday-Friday) of the given day. This is only available for Day_of_month. For example, if you specify 20W, and the 20th is a Saturday, the class runs on the 19th. If you specify 1W, and the first is a Saturday, the class does not run in the previous month, but on the third, which is the following Monday. Use the L and W together to specify the last weekday of the month.|
|#|Specifies the nth day of the month, in the format weekday#day_of_month. This is only available for Day_of_week. The number before the # specifies weekday (SUN-SAT). The number after the # specifies the day of the month. For example, specifying 2#2 means the class runs on the second Monday of every month.|

### Example Strings
|Expression|Description|
|--- |--- |
|0 0 13 * * ?|Class runs every day at 1 PM.|
|0 0 22 ? * 6L|Class runs the last Friday of every month at 10 PM.|
|0 0 10 ? * MON-FRI|Class runs Monday through Friday at 10 AM.|
|0 0 20 * * ? 2010|Class runs every day at 8 PM during the year 2010.|
