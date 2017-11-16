## url
GET {{url}}/bills?status=UNPAID

## tests
pm.test("Code de statut est 200", function() {
    pm.response.to.have.status(200);
});

var bill2 = JSON.parse(pm.environment.get("second-bill"));

var schema = {
    type: "array",
    minItems: 1,
    allOf: [bill2]
};

pm.test("Schéma est valide", function() {
    pm.expect(tv4.validate(pm.response.json(), schema)).to.be.true;
});

pm.environment.unset("first-bill");
pm.environment.unset("first-bill-id");
pm.environment.unset("first-client-id");
pm.environment.unset("second-bill");
pm.environment.unset("second-bill-id");
pm.environment.unset("second-client-id");