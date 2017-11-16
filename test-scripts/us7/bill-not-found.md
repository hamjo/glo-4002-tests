## url 
PUT {{url}}/bills/{{absent-bill}}

## prerequest
pm.environment.set("absent-bill", 0);

## body
{
	"amount": 2,
	"description": "lol"
}

## tests
pm.test("Status code is 404", function() {
    pm.response.to.have.status(404);
});

var schema = {
    properties: {
        maxproperties: 0
    }
};

pm.test("Schéma est valide", function() {
    pm.expect(tv4.validate(pm.response.json(), schema)).to.be.true;
});

pm.environment.unset("absent-bill");