## url
GET {{url}}/bills?clientId={{first-client-id}}

## tests
pm.test("Code de statut est 200", function() {
    pm.response.to.have.status(200);
});

var bill1 = JSON.parse(pm.environment.get("first-bill"));
var bill2 = JSON.parse(pm.environment.get("second-bill"));

var schema = {
    type: "array",
    minItems: 2,
    allOf: [bill1, bill2]
};

pm.test("Schéma est valide", function() {
    pm.expect(tv4.validate(pm.response.json(), schema)).to.be.true;
});