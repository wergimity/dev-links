* Kokia darbo patirtis, prie kokių projektų esi dirbes?
* Javascript patirtis.
* Ką teko daryti su moodle? Kuo skiriasi Moodle / Totara?
* Kiek žinai apie composer / packagist?
* Kaip reiktų saugoti slaptažodžius?
* Koks skirtumas tarp public / private / protected?
* Kaip laikyti / nelaikyti datas duomenų bazėje?
* Kokius darbo įrankius naudoji?
* Žinai XSS / CSRF / OWASP ?
* BDD žinios? BDD įrankiai? Behat?
* Laravel - kas tai su kuo tai valgoma?
* Kaip realizuotum validavimą Laravel'yje?
* Su kokia PHP versija tenka programuot?
* Kokius design patternus galėtum įvardinti arba kokius esi naudojęs?
```
Factory
Singleton
Adapter
Decorator
IOC / DependencyInjection
Facade / Proxy
Command
Strategy
Repository
```

Kas negerai?:
```php
<?php
/** PHP version: 5.4 */

abstract class User {
    protected $username;
    protected $email;
    protected $birthDate;
    protected $password;

    abstract function getBirthDate();
    abstract function setBirthDate($birthDate);
    abstract function getEmail();
    abstract function setEmail($email);
    abstract function getUsername();
    abstract function setUsername($username);
    abstract function setPassword($password);
    abstract function getPassword();
}

class UserService {
    /** @param User $user */
    public function updateInfo($user) {
        if(!$user instanceof User) {
            throw new \Exception('$user must be an instance of ' . User::class);
        }
        
        if(empty($user->getBirthDate())) {
            throw new \Exception('Users birth date cannot be empty');
        }


        $data = [
            'username' => $user->getUsername(),
            'email' => $user->getEmail(),
            'birth_date' => $user->getBirthDate(),
        ];

        return DB::table('users')->insert($data);
    }
}
```
