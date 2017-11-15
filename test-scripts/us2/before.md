## url
POST {{url}}/bills

## prerequest
var body = {
   clientId: 2,
   creationDate: "2017-08-21T16:59:20.142Z",
   dueTerm: "IMMEDIATE",
   items: []
};

pm.environment.set("body", JSON.stringify(body));

## body
{{body}}

## tests
pm.test("Code de statut est 201", function() {
    pm.response.to.have.status(201);
});

pm.environment.set("bill-id", pm.response.json().id);