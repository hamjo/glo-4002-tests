## url 
DELETE {{url}}/bills/{{absent-bill}}

## prerequest
pm.environment.set("absent-bill", 0);

## body
{}

## tests
pm.test("Code de statut est 404", function() {
    pm.response.to.have.status(404);
});

var schema = {
    properties: {
        maxProperties: 0
    }
};

pm.test("Contenu est valide", function() {
    pm.expect(tv4.validate(pm.response.json(), schema)).to.be.true;
});

pm.environment.unset("bill-id");
pm.environment.unset("absent-bill");