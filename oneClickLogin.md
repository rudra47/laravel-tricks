## route: 
```
Route::get('provider-dev-login', array('as'=>'providerDevLogin', 'uses'=>'MasterController@providerDevLogin'));
```

## Controller:  
```
public function providerDevLogin()
{
    //Check local environment or not
    abort_unless(app()->environment('local'), 404);
    //Login Auth Data
    $data = array(
        'email'          => 'support@iisbd.com',
        'password'       => '123456789',
        'email_verified' => 1,
        'status'         => 'Active',
        'valid'          => 1
    );
    Auth::guard('provider')->attempt($data);

    return redirect()->route('provider.apps');
}
```

## Blade: (put it to the login page)
```
@if (app()->environment('local'))
                            
<div class="form-group mb0 text-center">
    <p>Or Continue with </p>
    <div class="col-lg-12 col-md-12 col-sm-12 col-xs-12 mb25">
        <a href="{{route('provider.providerDevLogin')}}" class="btn btn-default">Developer Login</a>
    </div>
</div>

@endif
```