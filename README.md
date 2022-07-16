# laravel_login_register_v3

https://codeanddeploy.com/blog/laravel/laravel-8-authentication-login-and-registration-with-username-or-email

#### Set Route

```php
<?php

use App\Http\Controllers\HomeController;
use App\Http\Controllers\LoginController;
use App\Http\Controllers\LogoutController;
use App\Http\Controllers\RegisterController;
use Illuminate\Support\Facades\Route;


Route::group(['middleware' => ['guest']], function () {
    /**
     * Register Routes
     */
    Route::get('/register', [RegisterController::class, 'show'])->name('register.show');
    Route::post('/register/perform', [RegisterController::class, 'register'])->name('register.perform');

    /**
     * Login Routes
     */
    Route::get('/login', [LoginController::class, 'show'])->name('login.show');
    Route::post('/login', [LoginController::class, 'login'])->name('login.perform');
});

Route::group(['middleware' => ['auth']], function () {
    Route::get('/soon', [HomeController::class, 'index'])->name('soon');

    Route::get('/', [HomeController::class, 'index'])->name('home.index');

    Route::get('/logout', [LogoutController::class, 'perform'])->name('logout.perform');

    Route::prefix('home')->group(function () {
    });
});
```

---
#### Session
```php
<?php $data = Illuminate\Support\Facades\Session::get('success'); ?>
```

---
#### Set Home

- App\Providers\RouteServiceProvider.php

```php
public const HOME = '/';
```

---
#### Set App\Http\Middleware\Authenticate
```php
<?php

namespace App\Http\Middleware;

class Authenticate extends Middleware
{
    protected function redirectTo($request)
    {
        if (! $request->expectsJson()) {
            return route('login.show');
        }
    }
}
```

---
#### HTML

```html
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.0-beta1/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-0evHe/X+R7YkIZDRvuzKMRqM+OrBnVFBL6DOitfPri4tjfHxaWutUpFmBp4vmVor" crossorigin="anonymous">
```

---
#### Spesial Credential
```php
public function getCredentials()
{
    $username = $this->get('username');

    if ($this->isEmail($username)) {
        return [
            'email' => $username,
            'password' => $this->get('password'),
            'flag_active' => 1
        ];
    }

    $this->merge(['flag_active' => 1]);
    return $this->only('username', 'password', 'flag_active');
}
```
