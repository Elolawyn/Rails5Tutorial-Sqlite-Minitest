# Parte 4 - Modelando usuarios

[Volver al repositorio](https://github.com/Elolawyn/Rails5Tutorial) - [Parte 3 - Aspecto de la aplicación](https://github.com/Elolawyn/Rails5Tutorial/tree/master/docs/03/README.md)

En esta parte vamos a crear el modelo de usuario.

Modificar **Gemfile:**

```ruby
gem 'bcrypt', '3.1.11'
```

Ejecutar:

```bash
bundle install
rails generate model User name:string email:string
rails generate migration add_index_to_users_email
rails generate migration add_password_digest_to_users password_digest:string
```

Modificar **db/migrate/NUMERO_add_index_to_users_email.rb:**

```ruby
class AddIndexToUsersEmail < ActiveRecord::Migration[5.0]
  def change
    add_index :users, :email, unique: true
  end
end
```

Modificar **test/fixtures/users.yml:**

```YAML
# empty
```

Ejecutar:

```bash
rails db:migrate
```

Modificar **app/models/user.rb:**

```ruby
class User < ApplicationRecord
  before_save { email.downcase! }
  
  VALID_EMAIL_REGEX = /\A[\w+\-.]+@[a-z\d\-]+(\.[a-z\d\-]+)*\.[a-z]+\z/i
  
  has_secure_password
  
  validates :name,  presence: true, length: { maximum: 50 }
  validates :email, presence: true, length: { maximum: 255 }, format: { with: VALID_EMAIL_REGEX }, uniqueness: { case_sensitive: false }
  validates :password, presence: true, length: { minimum: 6 }
end
```

Modificar **test/models/user_test.rb:**

```ruby
require 'test_helper'

class UserTest < ActiveSupport::TestCase
 
  def setup
    @user = User.new(name: "Example User", email: "user@example.com", password: "foobar", password_confirmation: "foobar")
  end

  test "should be valid" do
    assert @user.valid?
  end
  
  test "name should be present" do
    @user.name = "     "
    assert_not @user.valid?
  end

  test "email should be present" do
    @user.email = "     "
    assert_not @user.valid?
  end

  test "name should not be too long" do
    @user.name = "a" * 51
    assert_not @user.valid?
  end

  test "email should not be too long" do
    @user.email = "a" * 244 + "@example.com"
    assert_not @user.valid?
  end
  
  test "email validation should accept valid addresses" do
    valid_addresses = %w[user@example.com USER@foo.COM A_US-ER@foo.bar.org first.last@foo.jp alice+bob@baz.cn]
    valid_addresses.each do |valid_address|
      @user.email = valid_address
      assert @user.valid?, "#{valid_address.inspect} should be valid"
    end
  end
  
  test "email validation should reject invalid addresses" do
    invalid_addresses = %w[user@example,com user_at_foo.org user.name@example. foo@bar_baz.com foo@bar+baz.com]
    invalid_addresses.each do |invalid_address|
      @user.email = invalid_address
      assert_not @user.valid?, "#{invalid_address.inspect} should be invalid"
    end
  end
  
  test "email addresses should be unique" do
    duplicate_user = @user.dup
    duplicate_user.email = @user.email.upcase
    @user.save
    assert_not duplicate_user.valid?
  end

  test "email addresses should be saved as lower-case" do
    mixed_case_email = "Foo@ExAMPle.CoM"
    @user.email = mixed_case_email
    @user.save
    assert_equal mixed_case_email.downcase, @user.reload.email
  end

  test "password should be present (nonblank)" do
    @user.password = @user.password_confirmation = " " * 6
    assert_not @user.valid?
  end

  test "password should have a minimum length" do
    @user.password = @user.password_confirmation = "a" * 5
    assert_not @user.valid?
  end

end
```

Revisar los ficheros para ver en qué consisten los tests y el modelo. Comprueba también las migraciones creadas y si quieres usa **DB Browser for SQLite** para comprobar la base de datos creada.

[Parte 5 - Registro de usuarios](https://github.com/Elolawyn/Rails5Tutorial/tree/master/docs/05/README.md)
