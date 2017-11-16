## url
POST {{url}}/bills

## prerequest
var body = {
   "clientId": 2,
   "creationDate": "2017-08-21T16:59:20.142Z",
   "dueTerm": "IMMEDIATE",
   "items": [
        {
           price: 5.00,
           note: "Item note 1",
           productId: 1,
           quantity: 1  
        }, 
        {
           price: 10.00,
           note: "Item note 2",
           productId: 2,
           quantity: 2
        }       
    ]
};

pm.environment.set("first-bill", JSON.stringify(body));

## body
{{first-bill}}

## tests
pm.test("Code de statut est 201", function() {
    pm.response.to.have.status(201);
});

var body = JSON.parse(pm.environment.get("first-bill"));

var bill = {
   id: pm.response.json().id,
   clientId: body.clientId,
   total: pm.response.json().total,
   items: []
};

for(let i = 0; i < body.items.length; i++){
    const item = body.items[i];
    item.total = item.quantity * item.price;
    item.amount = item.price;
    delete item.price;
    bill.items.push(item);
}

pm.environment.set("first-bill", JSON.stringify(bill));
pm.environment.set("first-bill-id", JSON.stringify(bill.id));
pm.environment.set("first-client-id", bill.clientId);