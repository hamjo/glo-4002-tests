## url 
POST {{url}}/bills

## prerequest
pm.environment.set("product-id", 0);

## body
{
   "clientId": 1,
   "creationDate": "2017-08-21T16:59:20.142Z",
   "dueTerm": "IMMEDIATE",
   "items": [
     {
       "price": 0.00,
       "note": "Item note",
       "productId": {{product-id}},
       "quantity": 0
     }
   ]
}	

## tests
pm.test('Status code is 400', function(){
    pm.response.to.have.status(400);
});

var schema = {
    properties: {
        errors: {
            type: "array",
            items: {
                type: "object",
                maxItems: 1,
                minItems: 1,
                properties: {
                    entity: {
                        type: "string",
                        pattern: "product"
                    },
                    error: {
                        type: "string",
                        pattern: "not found"
                    },
                    description: {
                        type: "string",
                        pattern : "product " + pm.environment.get("product-id") + " not found"
                    }
                },
                required: ["error", "description", "entity"]
            }
        }
    },
    required: ["errors"]
};

pm.test('Contenu est valide', function(){
    var data = pm.response.json();
    pm.expect(tv4.validate(data, schema)).to.be.true;
});


pm.environment.unset("product-id");

