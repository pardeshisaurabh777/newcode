<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SUNRISE</title>
</head>
<body>
    <head>
        <link rel="stylesheet" href="http://netdna.bootstrapcdn.com/bootstrap/3.0.0/css/bootstrap.min.css">
        <link rel="stylesheet" type="text/css" href="css/style.css">  
        <link rel='stylesheet' href='https://code.jquery.com/ui/1.11.4/themes/smoothness/jquery-ui.css'>
      </head>
      <body ng-app="invoicing" ng-controller="InvoiceCtrl" >
        <div class="container" width="800px" id="invoice" >
          <div class="row">
            <div class="col-xs-12 heading">
              INVOICE
            </div>
          </div>
          <div class="row branding">
            <div class="col-xs-6">
              <div class="invoice-number-container">
                <label for="invoice-number">Invoice #</label><input type="text" id="invoice-number" ng-model="invoice.invoice_number" /><br>
                <label for="invoice-number">Date </label> <input type="text" id="datepicker" />
              </div>
            </div>
            <div class="col-xs-6 logo-container">
              <input type="file" id="imgInp" />
              <img ng-hide="logoRemoved" id="company_logo" ng-src="{{ logo }}" alt="your image" width="300" />
              <div>
                <div class="noPrint" ng-hide="printMode">
                  <a ng-click="editLogo()" href >Edit Logo</a>
                  <a ng-click="toggleLogo()" id="remove_logo" href >{{ logoRemoved ? 'Show' : 'Hide' }} logo</a>
                </div>
              </div>
            </div>
          </div>
          <div class="row infos">
            <div class="col-xs-6">
              <div class="input-container"><input type="text" ng-model="invoice.customer_info.name"/></div>
              <div class="input-container"><input type="text" ng-model="invoice.customer_info.web_link"/></div>
              <div class="input-container"><input type="text" ng-model="invoice.customer_info.address1"/></div>
              <div class="input-container"><input type="text" ng-model="invoice.customer_info.address2"/></div>
              <div class="input-container"><input type="text" ng-model="invoice.customer_info.postal"/></div>
              <div class="input-container" data-ng-hide='printMode' style="visibility:hidden">
                <select ng-model='currencySymbol' ng-options='currency.symbol as currency.name for currency in availableCurrencies'></select>
              </div>
            </div>
            <div class="col-xs-6 right">
              <div class="input-container"><input type="text" ng-model="invoice.company_info.name" title="Customer Name" class="bor-bor"></div>
              <div class="input-container"><input type="text" ng-model="invoice.company_info.web_link" title="Customer Address line 1"></div>
              <div class="input-container"><input type="text" ng-model="invoice.company_info.address1" title="Customer Address line 2"></div>
              <div class="input-container"><input type="text" ng-model="invoice.company_info.address2" title="Customer Contact Number"></div>
              <div class="input-container"><input type="text" ng-model="invoice.company_info.postal" title="Customer Email Id"></div>
            </div>
          </div>
          <div class="items-table">
            <div class="row header">
              <div class="col-xs-1">&nbsp;</div>
              <div class="col-xs-5">Description</div>
              <div class="col-xs-2">Quantity</div>
              <div class="col-xs-2">Cost {{currencySymbol}}</div>
              <div class="col-xs-2 text-right">Total</div>
            </div>
            <div class="row invoice-item" ng-repeat="item in invoice.items" ng-animate="'slide-down'">
              <div class="col-xs-1 remove-item-container">
                <a href ng-hide="printMode" ng-click="removeItem(item)" class="btn btn-danger">[X]</a>
              </div>
              <div class="col-xs-5 input-container">
                <input ng-model="item.description" placeholder="Description" >
              </div>
              <div class="col-xs-2 input-container">
                <input ng-model="item.qty" value="1" size="4" ng-required ng-validate="integer" placeholder="Quantity" />
              </div>
              <div class="col-xs-2 input-container">
                <input ng-model="item.cost" value="0.00" ng-required ng-validate="number" size="6" placeholder="Cost" />
              </div>
              <div class="col-xs-2 text-right input-container">
                {{item.cost * item.qty | currency: currencySymbol}}
              </div>
            </div>
            <div class="row invoice-item">
              <div class="col-xs-12 add-item-container" ng-hide="printMode">
                <a class="btn btn-primary" href ng-click="addItem()" >[+]</a>
              </div>
            </div>
            <div class="row">
              <div class="col-xs-10 text-right">Sub Total</div>
              <div class="col-xs-2 text-right">{{invoiceSubTotal() | currency: currencySymbol}}</div>
            </div>
            <div class="row">
              <div class="col-xs-10 text-right">GST(%): <input ng-model="invoice.tax" ng-validate="number" style="width:25px"></div>
              <div class="col-xs-2 text-right">{{calculateTax() | currency: currencySymbol}}</div>
            </div>
            <div class="row">
              <div class="col-xs-10 text-right">Grand Total:</div>
              <div class="col-xs-2 text-right">{{calculateGrandTotal() | currency: currencySymbol}}</div>
            </div>
          </div>
          <div class="row noPrint actions">
            <a href="#" class="btn btn-primary" ng-show="printMode" ng-click="printInfo()">Print</a>
            <a href="#" class="btn btn-primary" ng-click="clearLocalStorage()">Reset</a>
            <a href="#" class="btn btn-primary" onclick="myFunction()" >Print</a>
          </div>
          <div class="row">
              <section class="signature">
                <div>
                  <p>Sheetal Agencies</p>
                  <p><p>
                </div>
              </section>
          </div>
        </div>
        
      <script>
      function myFunction() {
          window.print();
      }
      </script>
      <script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/angularjs/1.4.4/angular.min.js"></script>
      <script type="text/javascript" src="js/main.js"></script>
      <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>
      <script src="http://code.jquery.com/ui/1.11.4/jquery-ui.js"></script>
      <script>
      $(function() {
          $("#datepicker" ).datepicker({
              dateFormat: 'dd / mm / yy'	});
      
        } );
      
      var date = new Date();
      
      var day = date.getDate();
      var month = date.getMonth() + 1;
      var year = date.getFullYear();
      
      if (month < 10) month = "0" + month;
      if (day < 10) day = "0" + day;
      
      var today = day + " / " + month + " / " + year;
      
      
      document.getElementById('datepicker').value = today;
      </script>
      
</body>
<style>
    @import url('https://fonts.googleapis.com/css?family=Roboto+Slab:400,700|Rubik:400,500,700');
/* 
font-family: 'Rubik', sans-serif;
font-family: 'Roboto Slab', serif;
*/
*{
	font-family: 'Rubik', sans-serif !important;
}

.slide-down-enter,
.slide-down-leave
{
    -webkit-transition:200ms cubic-bezier(0.250, 0.250, 0.750, 0.750) all;
    -moz-transition:200ms cubic-bezier(0.250, 0.250, 0.750, 0.750) all;
    -ms-transition:200ms cubic-bezier(0.250, 0.250, 0.750, 0.750) all;
    -o-transition:200ms cubic-bezier(0.250, 0.250, 0.750, 0.750) all;
    transition:200ms cubic-bezier(0.250, 0.250, 0.750, 0.750) all;
    display:block;
    overflow:hidden;
    position:relative;
}

.items-table .row {
  border-bottom:1px solid #ddd;
  line-height:3em;
}
.items-table .row:last-child {
  border-bottom:none;
  line-height:3em;
}

.slide-down-enter.slide-down-enter-active,
.slide-down-leave {
    opacity:1;
    height:46px;
}

.slide-down-leave.slide-down-leave-active,
.slide-down-enter {
    opacity:0;
    height:0px;
}


.invoice-number-container * {
  font-weight:400;
}

.items-table .row:nth-child(even) {
  background:#f9f9f9;
}
.items-table input {
  line-height:1.5em;
}
.actions {
  padding-top:1em;
}
input:focus {
  outline: 0;
}
.col-xs-5.input-container input {
    width: 100%;
}
.heading {
  background-color:#357EBD;
  color:#FFF;
  margin-bottom:1em;
  text-align:center;
  line-height:2.5em;
}
.branding {
  padding-bottom:2em;
  border-bottom:1px solid #ddd;
}
.logo-container {
  text-align:right;
}
.infos .right {
  text-align:right;
}
.infos .right input {
  text-align:right;
}
.infos .input-container {
  padding:3px 0;
}

.header.row {
  font-weight:bold;
  border-bottom:1px solid #ddd;
  border-top:1px solid #ddd;
}

input, textarea{
  border: 1px solid #FFF; 
}

.container input:hover, .container textarea:hover,
.table-striped > tbody > tr:nth-child(2n+1) > td input:hover,
.container input:focus, .container textarea:focus,
.table-striped > tbody > tr:nth-child(2n+1) > td input:focus{
  border: 1px solid #CCC;
}

.table-striped > tbody > tr:nth-child(2n+1) > td input{
    background-color: #F9F9F9;
    border: 1px solid #F9F9F9;
}


.signature p {
    font-size: 13px;
    font-family: 'Roboto Slab', serif;
    letter-spacing: 1px;
    line-height: 1.4 !important;
    margin-top: 80px;
}
@media print {
    .noPrint {
        display:none;
    }
    .remove-item-container{
        visibility:hidden;
    }
    .add-item-container{
        visibility:hidden;
    }
	* { -webkit-print-color-adjust: exact; }
		html { background: none; padding: 0; }
		body { box-shadow: none; margin: 0; }
		span:empty { display: none; }
		.add, .cut { display: none; }
	  	.central_btn{display: none;}

}


body{
  padding:20px;
}

.infos input{
  width: 300px;
}

.align-right input{
  text-align:right;
  width: 300px;
}

div.container{
  width: 800px;
}

#imgInp{
  display: none;
}

.copy {
  font-family: "Lucida Grande", "Lucida Sans Unicode", "Lucida Sans", Geneva, Verdana, sans-serif;
  width: 100%;
  margin: 40px 0 20px 0;
  font-size: 10px;
  color: rgba(0, 0, 0, 0.5);
  text-align: center;
  color: #404040;
  cursor: default;
  line-height: 1.4em;
}

.copy .love {
  display: inline-block;
  position: relative;
  color: #ce0c15;
}


.ui-datepicker .ui-datepicker-title span.ui-datepicker-month {
    font-family: 'Rubik', sans-serif;
    font-weight: 400;
    font-size: 15px;
}
.ui-datepicker .ui-datepicker-title span.ui-datepicker-year {
    font-family: 'Rubik', sans-serif;
    font-weight: 400;
    font-size: 15px;
}
th .ui-datepicker-week-end {
    font-family: 'Rubik', sans-serif;
    font-weight: 400;
    font-size: 15px;
    padding-top: 12px;
}
.ui-datepicker table th {
    font-family: 'Rubik', sans-serif;
    font-weight: 400;
    font-size: 13px;
}
.ui-datepicker td {
    font-size: 13px;
    font-family: 'Rubik', sans-serif;
}
.ui-datepicker td a.ui-state-default.ui-state-highlight.ui-state-active {
    background: #ffffff;
    color: #3F51B5;
    border: 1px solid #3F51B5;
    text-align: center;
}
.ui-datepicker .ui-datepicker-prev, .ui-datepicker .ui-datepicker-next {
    position: absolute;
    top: -4px;
    width: 1.8em;
    height: 1.8em;
}
.ui-datepicker table thead tr th:nth-child(2) {
    background: #F44336;
    color: #fff;
}
.ui-datepicker th {
    padding: 5px;
    text-align: center;
    font-weight: bold;
    border: 0;
    border-radius: 0;
}
.ui-datepicker table tbody tr td:nth-child(2) a.ui-state-default {
    border: 1px solid #f44336;
    text-align: center;
    background: #f44336;
    color: #fff;
}
.ui-state-default, .ui-widget-content .ui-state-default, .ui-widget-header .ui-state-default {
    border: 1px solid #00BCD4;
    background: #00BCD4 50% 50% repeat-x;
    font-weight: normal;
    color: #ffffff;
    text-align: center;
}
.ui-datepicker table th {
    font-family: 'Rubik', sans-serif;
    font-weight: 400;
    font-size: 13px;
    background: #3c3c3c;
    color: #fff;
    border-right: 2px solid #fff;
}
.ui-datepicker .ui-datepicker-header {
    position: relative;
    padding: .2em 0;
    border: none;
    background: none;
}
.ui-datepicker table tbody tr a.ui-state-default.ui-state-active {
    background: #fff !important;
    color: #00bcd4 !important;
}
.ui-state-hover, .ui-widget-content .ui-state-hover, .ui-widget-header .ui-state-hover, .ui-state-focus, .ui-widget-content .ui-state-focus, .ui-widget-header .ui-state-focus {
    border: none;
    background: none;
    font-weight: normal;
    color: #FFC107;
}
</style>
<script>
    angular.module('invoicing', [])

// The default logo for the invoice
.constant('DEFAULT_LOGO', 'http://txt-dynamic.static.1001fonts.net/txt/dHRmLjcyLjA2NWNkYi5UV0ZvYVc1a2NtRnJZWElnUVdkbGJtTjUuMAAA/youre-so-cool.regular.png')

// The invoice displayed when the user first uses the app
.constant('DEFAULT_INVOICE', {
  tax: 18.00,
  invoice_number: 1001,
  customer_info: {
    name: 'Mahindrakar Agency',
    web_link: 'Near Arya Samaj Mandir',
    address1: 'Udgir-413517',
    address2: 'Mo.No (+91) 989-014-9128',
    postal: 'Mo.No (+91) 989-013-9867'
  },
  company_info: {
    name: '',
    web_link: '',
    address1: '',
    address2: '',
    postal: ''
  },
  items:[
    { qty: 1, description: 'Switching Machine', cost: 4500 }
  ]
})

// Service for accessing local storage
.service('LocalStorage', [function() {

  var Service = {};

  // Returns true if there is a logo stored
  var hasLogo = function() {
    return !!localStorage['logo'];
  };

  // Returns a stored logo (false if none is stored)
  Service.getLogo = function() {
    if (hasLogo()) {
      return localStorage['logo'];
    } else {
      return false;
    }
  };

  Service.setLogo = function(logo) {
    localStorage['logo'] = logo;
  };

  // Checks to see if an invoice is stored
  var hasInvoice = function() {
    return !(localStorage['invoice'] == '' || localStorage['invoice'] == null);
  };

  // Returns a stored invoice (false if none is stored)
  Service.getInvoice = function() {
    if (hasInvoice()) {
      return JSON.parse(localStorage['invoice']);
    } else {
      return false;
    }
  };

  Service.setInvoice = function(invoice) {
    localStorage['invoice'] = JSON.stringify(invoice);
  };

  // Clears a stored logo
  Service.clearLogo = function() {
    localStorage['logo'] = '';
  };

  // Clears a stored invoice
  Service.clearinvoice = function() {
    localStorage['invoice'] = '';
  };

  // Clears all local storage
  Service.clear = function() {
    localStorage['invoice'] = '';
    Service.clearLogo();
  };

  return Service;

}])

.service('Currency', [function(){

  var service = {};

  service.all = function() {
    return [
      {
        name: 'Indian Rupee (₹)',
        symbol: '(₹)'
      }
    ]
  }

  return service;
  
}])

// Main application controller
.controller('InvoiceCtrl', ['$scope', '$http', 'DEFAULT_INVOICE', 'DEFAULT_LOGO', 'LocalStorage', 'Currency',
  function($scope, $http, DEFAULT_INVOICE, DEFAULT_LOGO, LocalStorage, Currency) {

  // Set defaults
  $scope.currencySymbol = '₹ ';
  $scope.logoRemoved = false;
  $scope.printMode   = false;

  (function init() {
    // Attempt to load invoice from local storage
    !function() {
      var invoice = LocalStorage.getInvoice();
      $scope.invoice = invoice ? invoice : DEFAULT_INVOICE;
    }();

    // Set logo to the one from local storage or use default
    !function() {
      var logo = LocalStorage.getLogo();
      $scope.logo = logo ? logo : DEFAULT_LOGO;
    }();

    $scope.availableCurrencies = Currency.all();

  })()
  // Adds an item to the invoice's items
  $scope.addItem = function() {
    $scope.invoice.items.push({ qty:1, cost:0, description:"" });
  }

  // Toggle's the logo
  $scope.toggleLogo = function(element) {
    $scope.logoRemoved = !$scope.logoRemoved;
    LocalStorage.clearLogo();
  };

  // Triggers the logo chooser click event
  $scope.editLogo = function() {
    // angular.element('#imgInp').trigger('click');
    document.getElementById('imgInp').click();
  };

  $scope.printInfo = function() {
    window.print();
  };

  // Remotes an item from the invoice
  $scope.removeItem = function(item) {
    $scope.invoice.items.splice($scope.invoice.items.indexOf(item), 1);
  };

  // Calculates the sub total of the invoice
  $scope.invoiceSubTotal = function() {
    var total = 0.00;
    angular.forEach($scope.invoice.items, function(item, key){
      total += (item.qty * item.cost);
    });
    return total;
  };

  // Calculates the tax of the invoice
  $scope.calculateTax = function() {
    return (($scope.invoice.tax * $scope.invoiceSubTotal())/100);
  };

  // Calculates the grand total of the invoice
  $scope.calculateGrandTotal = function() {
    saveInvoice();
    return $scope.calculateTax() + $scope.invoiceSubTotal();
  };

  // Clears the local storage
  $scope.clearLocalStorage = function() {
    var confirmClear = confirm('Are you sure you would like to clear the invoice?');
    if(confirmClear) {
      LocalStorage.clear();
      setInvoice(DEFAULT_INVOICE);
    }
  };

  // Sets the current invoice to the given one
  var setInvoice = function(invoice) {
    $scope.invoice = invoice;
    saveInvoice();
  };

  // Reads a url
  var readUrl = function(input) {
    if (input.files && input.files[0]) {
      var reader = new FileReader();
      reader.onload = function (e) {
        document.getElementById('company_logo').setAttribute('src', e.target.result);
        LocalStorage.setLogo(e.target.result);
      }
      reader.readAsDataURL(input.files[0]);
    }
  };

  // Saves the invoice in local storage
  var saveInvoice = function() {
    LocalStorage.setInvoice($scope.invoice);
  };

  // Runs on document.ready
  angular.element(document).ready(function () {
    // Set focus
    document.getElementById('invoice-number').focus();

    // Changes the logo whenever the input changes
    document.getElementById('imgInp').onchange = function() {
      readUrl(this);
    };
  });

}])

</script>
</html>v
