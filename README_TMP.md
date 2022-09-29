# Simotel Laravel Connect Sample Application

This is a sample laravel application to demonstrate how you can connect to simotel in Laravel. 

## Preparing laravel and install simotel-laravel-connect package
#### Step 1: Install laravel with compser
First of all you must prepare a laravel application, if you have not, install laravel with composer:
```
composer create-project --prefer-dist laravel/laravel simotel-connect
```
#### Step 2: Install simotel-laravel-connect
install simotel-laravel-connect package with composer:
``` 
composer require nasimtelecom/simotel-laravel-connect
```

#### Step 3: Config
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
#### 

#### Step 4: Connect to Simotel Server with SimotelApi (SA)
You can use Simotel facade to connect to simotel:
```php
// app\Http\Controller\SimotelConnectCOntroller.php
$data = [
    
];
$res = Simotel::connect("pbx/users/search",$data);
$users = $res->getData();

```
#### Step 5: Listen for SimotelEventApi (SEA)
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
#### Step 6: Listen to Simotel SmartApi

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