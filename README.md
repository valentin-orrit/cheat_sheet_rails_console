# Cheat-sheet Rails console

## New App with postgresql & Javascript
```Shell
$ rails new myapp -j esbuild -d postgresql
```
```Shell
$ bundle install
```
create database :
```Shell
$ rails db:create
```
run server :
```Shell
$ rails server
```
## Generate Model
must be **PascalCase**, **singular**.
```Shell
$ rails g model User
```
Or :
```Shell
$ rails g model User name:string age:integer #etc.
```
-> Create migration.
### link models for linked tables
```ruby
class Article < ApplicationRecord
  belongs_to :user
end
```
```ruby
class User < ApplicationRecord
  has_many :articles
end
```
> [!IMPORTANT]
> belongs_to => singular / has_many => plural
### `has_many...through` for Many to Many Relations
```ruby
class Doctor < ApplicationRecord
  has_many :appointments
  has_many :patients, through: :appointments
end
```
+ *can also use `has_and_belongs_to_many` instead if the join table doesnt have a model.*
+ *for One to One relations use `has_one`.*

## Migrations
tables must be **snake_case**, **plural** of Model.
```Shell
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
```Shell
$ rails db:migrate
```
Check migration status :
```Shell
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

### Print with table_print gem
```Shell
tp ModelName.limit(n), "col1", "col2" etc.
```

## Seed
In `db/seeds.rb`, same commands as in rails console :
```ruby
User.create(first_name: "jean", email:"jean@jean.jean")
User.create(first_name: "paul", email:"paul@paul.paul")
puts "Deux utilisateurs ont été créés"
```
```Shell
$ rails db:seed
```
Loops : 
```ruby
100.times do |index|
  User.create(first_name: "Nom#{index}", email: "email#{index}@example.com")
end
```
Reset database :
```Shell
$ rails db:reset #equivalent of db:drop db:create db:migrate db:seed
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