## url
POST {{url}}/bills/{{bill-id}}

## test
pm.test('Status code is 200', function(){
    pm.response.to.have.status(200);
});

var requestBody = pm.environment.get("body");

var schema = {
    required: ["id", "effectiveDate", "expectedPayment", "dueTerm", "url"],
    properties: {
        id: {
            type: "integer"
        },
        effectiveDate: {
            type: "string",
            pattern: requestBody.creationDate
        },
        expectedPayment: {
            type: "string",
            pattern: "[0-9]{4}-[0-9]{2}-[0-9]{2}T[0-9]{2}:[0-9]{2}:[0-9]{2}\.[0-9]{3}Z"
        },
        dueTerm: {
            type: "string",
            pattern: requestBody.dueTerm    
        },
        url: {
            type: "string",
            pattern: "/bills/" + pm.response.json().id
        }
    },
};

pm.test('Contenu est valide', function(){
    var data = pm.response.json();
    pm.expect(tv4.validate(data, schema)).to.be.true;
});

pm.environment.unset("body");