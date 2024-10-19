```
# this project is a poc of https://github.com/akaunting/laravel-firewall
composer require akaunting/laravel-firewall

php artisan vendor:publish --tag=firewall

php artisan migrate



# other firewall https://github.com/terrylinooo/shieldon?tab=readme-ov-file
composer require shieldon/shieldon ^2
# In your bootstrap/app.php, after <?php, add the following code.
/*
|--------------------------------------------------------------------------
| Run The Shieldon Firewall
|--------------------------------------------------------------------------
|
| Shieldon Firewall will watch all HTTP requests coming to your website.
| Running Shieldon Firewall before initializing Laravel will avoid possible
| conflicts with Laravel's built-in functions.
*/
if (isset($_SERVER['REQUEST_URI'])) {

    // This directory must be writable.
    // We put it in the `storage/shieldon_firewall` directory.
    $storage =  __DIR__ . '/../storage/shieldon_firewall';

    $firewall = new \Shieldon\Firewall\Firewall();
    $firewall->configure($storage);

    // The base url for the control panel.
    $firewall->controlPanel('/firewall/panel/');
    $response = $firewall->run();

    if ($response->getStatusCode() !== 200) {
        $httpResolver = new \Shieldon\Firewall\HttpResolver();
        $httpResolver($response);
    }
}

#  Define a route for firewall panel.
Route::any('/firewall/panel/{path?}', function() {

    $panel = new \Shieldon\Firewall\Panel();
    $panel->csrf(['_token' => csrf_token()]);
    $panel->entry();

})->where('path', '(.*)');


open http://app.com/firewall/panel
user: shieldon_user
pass: shieldon_pass

```