## url 
POST {{url}}/payments

## prerequest
pm.environment.set("absent-client", 23124);

## body
	{
     "clientId": {{absent-client}},
     "amount": 2.00,
     "paymentMethod": {
       "account": "1525-154-451",
       "source": "CHECK"
     }
 }

## tests
pm.test("Code de statut est 400", function() {
    pm.response.to.have.status(400);
});


var schema = {
    required: ["errors"],
    properties: {
        errors: {
            type: "array",
            items: {
                type: "object",
                maxItems: 1,
                minItems: 1,
                required: ["error", "description", "entity"],
                properties: {
                    minProperties: 3,
                    maxProperties: 3,
                    entity: {
                        type: "string",
                        pattern: "client"
                    },
                    error: {
                        type: "string",
                        pattern: "not found"
                    },
                    description: {
                        type: "string",
                        pattern : "client " + pm.environment.get("absent-client") + " not found"
                    }
                },
            }
        }
    },
};
    

pm.test("Schéma est valide", function() {
    pm.expect(tv4.validate(pm.response.json(), schema)).to.be.true;
});

pm.environment.unset("absent-client");
