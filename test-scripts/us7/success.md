## url
PUT {{url}}/bills/{{bill-id}}

## body
{
	"amount": 10.5,
	"description": "a description"
}

## tests
pm.test("status code is 200", function() {
    pm.response.to.have.status(200);
});

var schema = {
   properties: {
       maxProperties: 0
   } 
};

pm.test("Schéma valide", function() {
    pm.expect(tv4.validate(data, pm.response.json())).to.be.true;
});
