angular.module('services.cart', []).service('Cart', ['$scope', 'Reviewer', function ($scope, Reviewer) { 

  var cart;

  $scope.getCart = function() {
    var cartVal =  localStorage.getItem('cart');
    cart = JSON.parse( cartVal );
    return cart;
  };

  $scope.addItem = function (item) {
    cart = $scope.getCart();
    cart[item.Id] = item.quantity;
    $scope.save(cart);
  };

  $scope.addItems = function(items) {
    items.forEach(function(obj){
      $scope.addItem(obj);
    });
  };

  $scope.save = function(cart) {
    Reviewer.review(cart).then(function(success){
      if(success){
        $scope.persist(cart);
      }
    });
  };

  $scope.remove = function (id) {
    cart = $scope.getCart();
    delete cart[id];
    $scope.save(cart);
  };

  $scope.clear = function() {
    if(localStorage.getItem('cart') != ''){
      localStorage.removeItem('cart');
      $scope.refresh();
    }
  };

  $scope.persist = function(cart) {
    localStorage.setItem('cart', JSON.stringify( cart ) );
    $scope.refresh();
  };

  $scope.changeQuantity = function (upatedQunatity,id){
    cart = this.getCart();
    cart[id] = updatedQuantity;
    $scope.save(cart);
  };

  $scope.refresh = function() {
    $scopecope.$apply();
  };
}]);
