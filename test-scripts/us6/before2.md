## url
POST {{url}}/bills

## prerequest
var body = {
   "clientId": 2,
   "creationDate": "2017-08-21T16:59:20.142Z",
   "dueTerm": "DAYS30",
   "items": [
        {
           price: 5.10,
           note: "Item note 3",
           productId: 3,
           quantity: 1  
        }
    ]
};

pm.environment.set("second-bill", JSON.stringify(body));

## body
{{second-bill}}

## tests
pm.test("Code de statut est 201", function() {
    pm.response.to.have.status(201);
});

var body = JSON.parse(pm.environment.get("second-bill"));

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

pm.environment.set("second-bill", JSON.stringify(bill));
pm.environment.set("second-client-id", bill.clientId);
pm.environment.set("second-bill-id", bill.id);