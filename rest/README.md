# REST Class
This is a snippet for making a call to a REST API End point, deserialising the JSON response and writing Mock and Test Classes.

> Read more on REST Apex Classes on [Admin2Dev](https://admin2dev.com/apex-starter/22.html)

## JSON

### JSON with an Object and key-value pairs
```
{
  	"bird": {
  		"id": 1,
  		"name": "chicken",
  		"food": "wheat",
  		"product": "eggs"
  	}
  }
```
### JSON Class

```
public class JSONResponse{
  public Bird bird; //Object
}

public class Bird { //object class
  //key value pairs

  public Integer id;
  public String name, food, product;

}
```

### JSON With Object, key-value pairs and Array

```
{
	"Name": {
		"firstName": "John",
		"lastName": "Smith"
	},
	"Deals": [
		{
			"Name": "Opportunity Name",
			"CloseDate": "03022020",
			"Amount": 2200
		},
		{
			"Name": "Opportunity 2 Name",
			"CloseDate": "03122020",
			"Amount": 3000
		},
		{
			"Name": "Opportunity 3 Name",
			"CloseDate": "12062020",
			"Amount": 800
		}
	],
	"Account": "GenePoint"
}
```

### JSON Class
```
public class JSONResponse{
	public String Account; //key-value
	public Name name; //Object name
	public List<Deals> deals; //Array
}

public class Name{ //Object Class
	public String firstName, lastName;
}

public class Deals{ //Array class
	public String Name, CloseDate;
	public Integer Amount;
}
```

## Apex REST Class

### Snippet

```
public class httpResponseClass {

      public class JSONResponse { //create JSON structure class
      	//Define values as is with data types
      	//Ensure name of the variables matches the JSON keys
      }

  	public static String methodName() {

          //Make the Request and save it

          String requestURL = 'http://url-goes-here.com/'; //End Point URL goes here
          String requestType = 'GET'; //GET POST PATCH PUT DELETE

          Http http = new Http();
          HttpRequest request = new HttpRequest();
          request.setEndpoint(requestURL);
          request.setMethod(requestType);
          HttpResponse response = http.send(request);

          // Deserialize based on result type class
          JSONResponse result = (JSONResponse) JSON.deserialize(response.getBody(), JSONResponse.class);
          return result.JSONObject.Key_Name; //Replace JSONObject with value from JSONResponse and Key_Name with the key you're accessing in the said JSON

     }
  }
```

## Example

### Example JSON
```
{
	"Name": {
		"firstName": "John",
		"lastName": "Smith"
	},
	"Deals": [
		{
			"Name": "Opportunity Name",
			"CloseDate": "03022020",
			"Amount": 2200
		},
		{
			"Name": "Opportunity 2 Name",
			"CloseDate": "03122020",
			"Amount": 3000
		},
		{
			"Name": "Opportunity 3 Name",
			"CloseDate": "12062020",
			"Amount": 800
		}
	],
	"Account": "GenePoint"
}
```

### Example Class

```
public class ResponseClass {

    public class JSONResponse { //create JSON structure class
        public Name name;
        public List<Deals> deals;
        public String Account;
    }

    public class Name{
        public String firstName, lastName;
    }
    public class Deals{
        public String name, CloseDate;
        public Integer amount;
    }

    public static Integer methodName() {

        //Make the Request and save it

        String requestURL = 'https://admin2dev.com/rest/response3.json'; //End Point URL goes here
        String requestType = 'GET'; //GET POST PATCH PUT DELETE

        Http http = new Http();
        HttpRequest request = new HttpRequest();
        request.setEndpoint(requestURL);
        request.setMethod(requestType);
        HttpResponse response = http.send(request);

        // Deserialize based on result type class
        JSONResponse result = (JSONResponse) JSON.deserialize(response.getBody(), JSONResponse.class);
        return result.Deals[1].Amount; //getting second amount from the array

    }
}
```

## Mock Class
```
@isTest
global class ClassNameMock implements HttpCalloutMock {
	global HTTPResponse respond(HTTPRequest request) {

        HttpResponse response = new Httpresponse();
        String responseJSON = 'JSON'; //Add your JSON in one line here

        response.setStatusCode(200);
        response.setBody(responseJSON);

        return response;
    }

}
```

## Test Class
```
@isTest
public class ClassNameTest {
    static testmethod void testerClass(){

        Test.setMock(HttpCalloutMock.class, new ComplexResponseClassMock()); //Make the call to mock response class

        //No Test.start / end necessary

        System.assertEquals(expected, ClassName.methodName()); //Verify the values are correct

    }

}
```
