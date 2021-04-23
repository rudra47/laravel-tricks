#firstOrNew
The firstOrNew method is really useful for finding the first Model that matches some constraints or making a new one if there isn’t one that matches those constraints.

You can take a piece of code that looks like this:

$user = User::where('email', request('email'))->first();

if ($user === null) {
    $user = new User(['email' => request('email')]);
}

$user->name = request('name');

$user->save()

And turn it into this:

$user = User::firstOrNew(['email' =>  request('email')]);

$user->name = request('name');

$user->save();

You may also pass an array of additional attributes to set as the second parameter if no existing Model is found:

$user = User::firstOrNew(
    ['email' =>  request('email')],
    ['name' => request('name')]
);

$user->save();

#firstOrCreate
The firstOrCreate method is very similar to the firstOrNew method. It tries to find a model matching the attributes you pass in the first parameter. If a model is not found, it automatically creates and saves a new Model after applying any attributes passed in the second parameter:

$user = User::firstOrCreate(
    ['email' =>  request('email')],
    ['name' => request('name')]
);

// No call to $user->save() needed

#firstOr
I recently found the firstOr method while source-diving. The firstOr method retrieves the first Model from a query, or if no matching Model is found, it will call a callback passed. This can be really useful if you need to perform extra steps when creating a user or want to do something other than creating a new user:

$user = User::where('email', request('email'))->firstOr(function () {
    $account = Account::create([ //... ]);

    return User::create([
        'account_id' => $account->id,
        'email' => request('email'),
    ]);
});

#updateOrCreate
The updateOrCreate method attempts to find a Model matching the constraints passed as the first parameter. If a matching Model is found, it will update the match with the attributes passed as the second parameter. If no matching Model is found a new Model will be created with both the constraints passed as the first parameter and the attributes passed as the second parameter.

You can refactor this piece of code:

$user = User::where('email', request('email'))->first();

if ($user !== null) {
    $user->update(['name' => request('name')]);
} else {
    $user = User::create([
      'email' => request('email'),
      'name' => request('name'),
    ]);
}

// Do other things with the User

To this using the updateOrCreate method:

$user = User::updateOrCreate(
    ['email' =>  request('email')],
    ['name' => request('name')]
);

// Do other things with the User
