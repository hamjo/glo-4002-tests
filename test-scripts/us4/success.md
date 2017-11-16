## url 
DELETE {{url}}/bills/{{bill-id}}

## body
{}

## tests
pm.test("Code de statut est 202", function() {
    pm.response.to.have.status(202);
});

pm.test("Schéma est valide", function() {
    pm.expect(pm.response.body).to.be.undefined;
});