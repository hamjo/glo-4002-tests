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
pm.test("Code de statut est 201", function() {
    pm.response.to.have.status(201);
});

pm.environment.unset("items");
pm.environment.set("bill-id", pm.response.json().id);