# NuSOAP for PHP

Fork of NuSOAP fixed for PHP 5.4, 5.5, 5.6, 7.0 and 7.1 (tested).

All credits belongs to official author(s): http://nusoap.sourceforge.net.

## Install
```sh
composer require lawiet/nusoap
```

## Usage

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