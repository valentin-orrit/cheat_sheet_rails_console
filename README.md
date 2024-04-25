# Cheat-sheet Rails console

## Rails DBs
### Generate Model
must be **PascalCase**, **singular**.
```shell
$ rails g model User
```
Or :
```shell
$ rails g model User name:string age:integer #etc.
```

-> Create migration.
### Migrations
tables must be **snake_case**, **plural** of Model.
```shell
$ rails generate migration NomDeTaMigration
```
Example of migration :
```ruby
def change
  create_table :users do |t|
    t.string :name
    t.boolean :is_admin
    t.timestamps
    t.belongs_to :other_table, index: true #foreign key
  end
end
```
Or migration to add something to an existing table :
```ruby
def change
  add_reference :articles, :user, foreign_key: true
end
```
Run migration :
```shell
$ rails db:migrate
```
Check migration status :
```shell
$ rails db:migrate:status
```
## CRUD
### Create
```ruby
> martin = User.new(first_name: "Martin", email: "martion@gmail.com", is_admin: true)
> martin.save
```
Or :
```ruby
> martin = User.create(first_name: "Martin", email: "martion@gmail.com", is_admin: true)
```

### Read
```ruby
> u = User.find(3)
> david = User.find_by(first_name: 'David')
```
More complex researchs :
```ruby
> users = User.where(first_name: 'David', is_admin: true).order(created_at: :desc)
```

### Update
```ruby
> user_1 = User.find(1)
> user_1.first_name = "a new name"
> user_1.save
```
Or :
```ruby
> user_1 = User.find(1)
> user_1.update(first_name: "a new name")
```

### Delete
```ruby
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
In `db/seeds.rb`, same commands as in rails console :
```ruby
User.create(first_name: "jean", email:"jean@jean.jean")
User.create(first_name: "paul", email:"paul@paul.paul")
puts "Deux utilisateurs ont été créés"
```
```shell
$ rails db:seed
```
Loops : 
```ruby
100.times do |index|
  User.create(first_name: "Nom#{index}", email: "email#{index}@example.com")
end
```

### Faker https://github.com/faker-ruby
```ruby
require 'faker'
100.times do
  user = User.create!(first_name: Faker::Name.first_name, email: Faker::Internet.email)
end
```

> ## Links
> + https://api.rubyonrails.org/
> + https://guides.rubyonrails.org/v7.1/