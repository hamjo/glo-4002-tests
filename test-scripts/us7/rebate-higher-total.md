## url 
PUT {{url}}/bills/{{bill-id}}

## prerequest
pm.environment.set("amount", 5 + pm.environment.get("total"));
pm.environment.unset("total");

## body
{
	"amount": {{amount}},
	"description": "XD"
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
                        pattern: "amount"
                    },
                    error: {
                        type: "string",
                        pattern: "validation"
                    },
                    description: {
                        type: "string",
                        pattern : "rebate can't be higher than total of the invoice"
                    }
                }
            }
        }
    }
}

pm.test('Contenu est valide', function(){
    var data = pm.response.json();
    console.log(tv4.validate(data, schema));
    pm.expect(tv4.validate(data, schema)).to.be.true;
});

pm.environment.unset("amount");
pm.environment.unset("bill-id");
