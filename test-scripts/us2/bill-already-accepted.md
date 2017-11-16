## url 
POST {{url}}/bills/{{bill-id}}

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
                    minProperties: 3,
                    maxProperties: 3,
                    entity: {
                        type: "string",
                        pattern: "invoice"
                    },
                    error: {
                        type: "string",
                        pattern: "wrong status"
                    },
                    description: {
                        type: "string",
                        pattern : "Invoice already accepted"
                    }
                },
                required: ["error", "description", "entity"]
            }
        }
    },
    required: ["errors"]
}

pm.test('Contenu est valide', function(){
    var data = pm.response.json();
    pm.expect(tv4.validate(data, schema)).to.be.true;
});