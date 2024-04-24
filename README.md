# cheat_sheet_rails_console

## CRUD

### Create
```
> martin = User.new(first_name: "Martin", email: "martion@gmail.com", is_admin: true)
> martin.save
```
or
```
> martin = User.create(first_name: "Martin", email: "martion@gmail.com", is_admin: true)
```

### Read
```
> u = User.find(3)
> david = User.find_by(first_name: 'David')
```
Recherches plus complexes:
```
> users = User.where(first_name: 'David', is_admin: true).order(created_at: :desc)
```
### Update
```
> user_1 = User.find(1)
> user_1.first_name = "a new name"
> user_1.save
```
or
```
> user_1 = User.find(1)
> user_1.update(first_name: "a new name")
```

### Delete
```
> user_1 = User.find(1)
> user_1.destroy
```

### Quelques méthodes

+ `#all` : te ressort tous les éléments de ton model (array).
+ `#new` : crée une nouvelle instance de ton model.
+ `#save` : enregistre cette instance.
+ `#create` : crée et enregistre une instance de model.
+ `#update(attribute: value)` : met à jour l'attribute de cet objet / entrée.
+ `#last` : te retourne la dernière entrée créée dans le model.
+ `#first` : pareil mais pour la première.
+ `#find(x) : te retourne l'entrée BDD dont l'`id` vaut `x` (integer)._
+ `#find_by(attribute: value)` : te retourne le premier élément qui a value à l'attribute.
+ `#where(attribute: value)` : te retourne tous les éléments (dans un array) qui ont `value` à l'`attribute`.
+ `#destroy` : supprime de la BDD l'entrée en question.

### Seed
In `db/seeds.rb`, same commands as in rails console
```
User.create(first_name: "jean", email:"jean@jean.jean")
User.create(first_name: "paul", email:"paul@paul.paul")
puts "Deux utilisateurs ont été créés"
```
In terminal
```
$ rails db:seed
```
loops 
```
100.times do |index|
  User.create(first_name: "Nom#{index}", email: "email#{index}@example.com")
end
```
Faker
```
require 'faker'
100.times do
  user = User.create!(first_name: Faker::Name.first_name, email: Faker::Internet.email)
end
```


### Links

https://api.rubyonrails.org/
https://guides.rubyonrails.org/v7.1/