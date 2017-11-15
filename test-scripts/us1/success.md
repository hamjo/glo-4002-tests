## url 
POST {{url}}/bills

## prerequest
var items = [
    {
       price: 5.00,
       note: "Item note",
       productId: 1,
       quantity: 1  
    }, 
    {
       price: 10.00,
       note: "Item note",
       productId: 2,
       quantity: 2
    }
];

pm.environment.set("items", JSON.stringify(items));

pm.environment.set("total", 25);

## body
{
   "clientId": 2,
   "creationDate": "2017-08-21T16:59:20.142Z",
   "dueTerm": "IMMEDIATE",
   "items": {{items}}
}	

## tests
pm.test('Status code is 201', function(){
    pm.response.to.have.status(201);
    postman.setEnvironmentVariable("bill-id", pm.response.json().id);
});

var schema = {
    properties: {
        id: {
            type: "integer",
            allOf: [postman.getEnvironmentVariable("bill-id")]
        },
        total: {
            type: "number",
            allOf: [postman.getEnvironmentVariable("total")]
        },
        dueTerm:{
            type: "string",
            anyOf:["IMMEDIATE","DAYS30","DAYS90"]
        },
        url: {
            type: "string",
            pattern: "/bills/" + pm.environment.get("bill-id")
        }
    },
    required: ["id", "total", "dueTerm", "url"]
};

pm.test('Contenu est valide', function(){
    var data = pm.response.json();
    pm.expect(tv4.validate(data, schema)).to.be.true;
});

pm.environment.unset("bill-id");
pm.environment.unset("total");
pm.environment.unset("items");