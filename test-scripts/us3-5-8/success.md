## url 
POST {{url}}/payments

## body
{
     "clientId": 1,
     "amount": 2.00,
     "paymentMethod": {
       "account": "1525-154-451",
       "source": "CHECK"
     }
 }

 ## tests
pm.test("Code de statut est 201", function() {
    pm.response.to.have.status(201);
});


var id = pm.response.json().id;

var schema = {
    properties: {
        maxProperties: 2,
        minProperties: 2,
        required: ["id", "url"],
        id: {
            type: "integer"
        },
        url: {
            type: "string",
            pattern: "/payments/" + id
        }
    }
};
    

pm.test("Schéma est valide", function() {
    
    pm.expect(tv4.validate(pm.response.json(), schema)).to.be.true;
});
