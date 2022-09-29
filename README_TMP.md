# Simotel Laravel Connect Sample Application

This is a laravel sample application to demonstrate how you can connect to simotel in Laravel.  
We use **[simotel-laravel-connect](https://github.com/nasimtelecom/simotel-laravel-connect)** package to connect simotel and laravel together.

## Preparing laravel and install simotel-laravel-connect package
#### Step 1: install laravel with compser
First of all you must prepare a laravel application, if you haven't install it yet, install laravel with composer:
```
composer create-project --prefer-dist laravel/laravel simotel-connect
```
#### Step 2: install simotel-laravel-connect
install simotel-laravel-connect package with composer:
``` 
composer require nasimtelecom/simotel-laravel-connect
```

#### Step 3: config
Use artisan command to publish simotel connect config file in laravel config folder:
```.
php artisan vendor:publish --provider="NasimTelecom\Simotel\Laravel\SimotelLaravelServiceProvider"
```

```php
// config/laravel-simotel.php

[
    'smartApi' => [
        'apps' => [
            '*' => "\YourApp\SmartApiAppClass",
        ],
    ],
    'ivrApi' => [
        'apps' => [
            '*' => "\YourApp\IvrApiAppClass",
        ],
    ],
    'trunkApi' => [
        'apps' => [
            '*' => "\YourApp\TrunkApiApp",
        ],
    ],
    'extensionApi' => [
        'apps' => [
            '*' => "\YourApp\ExtensionApiAppClass",
        ],
    ],
    'simotelApi' => [
        'server_address' => 'http://yourSimotelServer/api/v4',
        'api_auth' => 'basic',  // simotel api authentication: basic,token,both
        'api_user' => 'apiUser',
        'api_pass' => 'apiPass',
        'api_key' => 'apiToken',
    ],
];
```
## Simote Api (SA) 
//about simotel api 

#### Step 1: create api acount in simotel
[image simotel web interface]

![alt text](https://github.com/nasimtelecom/laravel-connect-sample/blob/main/public/images/simotel.png?raw=true)

#### Step 2: edit simotel config file 
```php
// config/laravel-simotel.php

'simotelApi' => [
        'server_address' => 'http://yourSimotelServer/api/v4',
        'api_auth' => 'basic',  // simotel api authentication: basic,token,both
        'api_user' => 'apiUser',
        'api_pass' => 'apiPass',
        'api_key' => 'apiToken',
    ],
```
#### Step 3: connect to simotel
In your controller use `Simotel` facade to connect to simotel:
```php
// app\Http\Controller\SimotelConnectController.php
$data = [
    
];
$res = Simotel::connect("pbx/users/search",$data);
$users = $res->getData();

```
## Simotel Event Api (SEA)
Make a listener with artisan command:
```
php artisan make:listener CdrEventListener
```
000000 Listener with SimotelEvent in EventServiceProvider
```php
[]
```
now define route in api.php and dispatch event by this commands:
```php
// routes/api.php

Route::get("simotel/events",function(Request $request, $event){

        try {
           Simotel::eventApi()->dispatch($event,$request->all());
        } catch (\Exception $exception) {
            die("error: " . $exception->getMessage());
        }

});
```
## Simotel SmartApi

```php
// routes/api.php

Route::get("simotel/smartApi",function(Request $request){

    try {
        $respond = Simotel::smartApi($request->all())->toArray();
        return response()->json($respond);
    } catch (\Exception $exception) {
        die("error: " . $exception->getMessage());
    }

});
```