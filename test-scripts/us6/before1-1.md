## url 
POST {{url}}/bills/{{first-bill-id}}

## test
pm.test('Status code is 200', function(){
    pm.response.to.have.status(200);
});