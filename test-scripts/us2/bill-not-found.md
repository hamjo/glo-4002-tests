## url 
POST {{url}}/bills/{{bill-id}}

## prerequest
pm.environment.set("absent-bill", 0);

## tests
pm.test('Status code is 404', function(){
    pm.response.to.have.status(404);
});

var schema = {
    properties: {
        maxProperties: 0
    }
};

pm.test('Contenu est valide', function(){
    var data = pm.response.json();
    pm.expect(tv4.validate(data, schema)).to.be.true;
});

pm.environment.unset("absent-bill");
pm.environment.unset("bill-id");