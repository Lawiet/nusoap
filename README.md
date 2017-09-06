# NuSOAP for PHP

Fork of NuSOAP fixed for PHP 5.4+ and 7.x

Work in namespace (maintenance)

All credits belongs to official author(s): http://nusoap.sourceforge.net.

## Install
```sh
composer require lawiet/nusoap
```

## Usage Client

```php
// Config
$client = new NuSoapClient('http://localhost/nusoap/server.php?wsdl', 'wsdl');
$client->soap_defencoding = 'UTF-8';
$client->decode_utf8 = FALSE;
$error  = $client->getError();

// Calls
$result = $client->call($action, $data);

if ($client->fault) {
    echo "<h2>Fault</h2><pre>";
    print_r($result);
    echo "</pre>";
} else {
    $error = $client->getError();
    if ($error) {
        echo "<h2>Error</h2><pre>" . $error . "</pre>";
    } else {
        echo "<h2>Main</h2>";
        echo $result;
    }
}
```



## Usage Server

```php
// Create the server instance
$server = new NuSoapServer();

$server->configureWSDL('server', 'urn:server');

$server->wsdl->schemaTargetNamespace = 'urn:server';

//SOAP complex type return type (an array/struct)
$server->wsdl->addComplexType(
    'Person',
    'complexType',
    'struct',
    'all',
    '',
    array(
        'id_user' => array('name' => 'id_user', 'type' => 'xsd:int'),
        'fullname' => array('name' => 'fullname', 'type' => 'xsd:string'),
        'email' => array('name' => 'email', 'type' => 'xsd:string'),
        'level' => array('name' => 'level', 'type' => 'xsd:int')
    )
);

//first simple function
$server->register('hello',
			array('username' => 'xsd:string'),  //parameter
			array('return' => 'xsd:string'),  //output
			'urn:server',   //namespace
			'urn:server#helloServer',  //soapaction
			'rpc', // style
			'encoded', // use
			'Just say hello');  //description

//this is the second webservice entry point/function 
$server->register('login',
			array('username' => 'xsd:string', 'password'=>'xsd:string'),  //parameters
			array('return' => 'tns:Person'),  //output
			'urn:server',   //namespace
			'urn:server#loginServer',  //soapaction
			'rpc', // style
			'encoded', // use
			'Check user login');  //description

//first function implementation
function hello($username) {
        return 'Howdy, '.$username.'!';
}

//second function implementation 
function login($username, $password) {
        //should do some database query here
        // .... ..... ..... .....
        //just some dummy result
        return array(
		'id_user'=>1,
		'fullname'=>'John Reese',
		'email'=>'john@reese.com',
		'level'=>99
	);
}

$HTTP_RAW_POST_DATA = isset($HTTP_RAW_POST_DATA) ? $HTTP_RAW_POST_DATA : '';

$server->service($HTTP_RAW_POST_DATA);
```
