## url
GET {{url}}/bills?status=OVERDUE

## tests
pm.test("Code de statut est 200", function() {
    pm.response.to.have.status(200);
});

var bill1 = JSON.parse(pm.environment.get("first-bill"));

var schema = {
    type: "array",
    minItems: 1,
    allOf: [bill1]
};

pm.test("Schéma est valide", function() {
    pm.expect(tv4.validate(pm.response.json(), schema)).to.be.true;
});