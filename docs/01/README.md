# Parte 1 - Comienzo de la aplicación

[Volver al repositorio](https://github.com/Elolawyn/Rails5Tutorial)

En esta parte se va a crear la aplicación y se van a dar los primeros pasos del tutorial.

<div id='index'/>
### Índice

1. [Crear la aplicación](#seccion01)
2. [Gemas](#seccion02)
3. [Probar la aplicación](#seccion03)
4. [Páginas estáticas](#seccion04)
5. [Testing](#seccion05)
6. [Dinamismo](#seccion06)

<div id='seccion01'/>
### Crear la aplicación

[Volver al índice](#index)

Ejecutar:

```bash
rails _5.0.1_ new sample_app
cd sample_app/
```

<div id='seccion02'/>
### Gemas

[Volver al índice](#index)

Modificar **Gemfile:**

```ruby
source 'https://rubygems.org'

gem 'rails',        '5.0.1'
gem 'puma',         '3.4.0'
gem 'sass-rails',   '5.0.6'
gem 'uglifier',     '3.0.0'
gem 'coffee-rails', '4.2.1'
gem 'jquery-rails', '4.1.1'
gem 'turbolinks',   '5.0.1'
gem 'jbuilder',     '2.4.1'

group :development, :test do
  gem 'sqlite3', '1.3.12'
  gem 'byebug',  '9.0.0', platform: :mri
end

group :development do
  gem 'web-console',           '3.1.1'
  gem 'listen',                '3.0.8'
  gem 'spring',                '1.7.2'
  gem 'spring-watcher-listen', '2.0.0'
end

group :test do
  gem 'rails-controller-testing', '0.1.1'
  gem 'minitest-reporters',       '1.1.9'
  gem 'guard',                    '2.13.0'
  gem 'guard-minitest',           '2.4.4'
end

group :production do
  gem 'pg', '0.18.4'
end

# Windows does not include zoneinfo files, so bundle the tzinfo-data gem
gem 'tzinfo-data', platforms: [:mingw, :mswin, :x64_mingw, :jruby]
```

Ejecutar:

```bash
bundle install --without production
bundle update
```

<div id='seccion03'/>
### Probar la aplicación

[Volver al índice](#index)

Modificar **app/controllers/application_controller.rb:**

```ruby
def hello
  render html: "hello, world!"
end
```

Modificar **config/routes.rb:**

```ruby
root 'application#hello'
```

Ejecutar:

```bash
rails server
```

<div id='seccion04'/>
### Páginas estáticas

[Volver al índice](#index)

Ejecutar:

```bash
rails generate controller StaticPages home help
```

Ver los nuevos ficheros creados, visitar las páginas nuevas.

Modificar **app/views/static_pages/home.html.erb:**

```RHTML
<h1>Sample App</h1>
<p>
  This is the home page for the
  <a href="http://www.railstutorial.org/">Ruby on Rails Tutorial</a>
  sample application.
</p>
```

Modificar **app/views/static_pages/help.html.erb:**

```RHTML
<h1>Help</h1>
<p>
  Get help on the Ruby on Rails Tutorial at the
  <a href="http://www.railstutorial.org/help">Rails Tutorial help page</a>.
  To get help on this sample app, see the
  <a href="http://www.railstutorial.org/book"><em>Ruby on Rails Tutorial</em>
  book</a>.
</p>
```

<div id='seccion05'/>
### Testing

[Volver al índice](#index)

Ver los ficheros de test. Ejecutar:

```bash
rails test
```

Modificar **app/views/static_pages/help.html.erb:**

```ruby
test "should get about" do
  get static_pages_about_url
  assert_response :success
end
```

Ejecutar:

```bash
rails test
```

Realizar los cambios necesarios para que los tests sean verdes.

Crear **app/views/static_pages/about.html.erb:**

```RHTML
<h1>About</h1>
<p>
  The <a href="http://www.railstutorial.org/"><em>Ruby on Rails
  Tutorial</em></a> is a
  <a href="http://www.railstutorial.org/book">book</a> and
  <a href="http://screencasts.railstutorial.org/">screencast series</a>
  to teach web development with
  <a href="http://rubyonrails.org/">Ruby on Rails</a>.
  This is the sample application for the tutorial.
</p>
```

<div id='seccion06'/>
### Dinamismo

[Volver al índice](#index)

Modificar **test/controllers/static_pages_controller_test.rb:**

```ruby
require 'test_helper'

class StaticPagesControllerTest < ActionDispatch::IntegrationTest

  def setup
    @base_title = "Ruby on Rails Tutorial Sample App"
  end

  test "should get root" do
    get root_url
    assert_response :success
    assert_select "title", "Home | #{@base_title}"
  end

  test "should get home" do
    get static_pages_home_url
    assert_response :success
    assert_select "title", "Home | #{@base_title}"
  end
  
  test "should get help" do
    get static_pages_help_url
    assert_response :success
    assert_select "title", "Help | #{@base_title}"
  end
  
  test "should get about" do
    get static_pages_about_url
    assert_response :success
    assert_select "title", "About | #{@base_title}"
  end
  
  test "should get contact" do
    get static_pages_contact_url
    assert_response :success
    assert_select "title", "Contact | #{@base_title}"
  end

end
```

Modificar **app/views/layouts/application.html.erb:**

```RHTML
<!DOCTYPE html>
<html>
  <head>
    <title><%= yield(:title) %> | Ruby on Rails Tutorial Sample App</title>
    <%= csrf_meta_tags %>
    <%= stylesheet_link_tag    'application', media: 'all',
                                              'data-turbolinks-track': 'reload' %>
    <%= javascript_include_tag 'application', 'data-turbolinks-track': 'reload' %>
  </head>

  <body>
    <%= yield %>
  </body>
</html>
```

Modificar **app/views/layouts/application.html.erb:**

```RHTML
<% provide(:title, "Home") %>
<h1>Sample App</h1>
<p>
  This is the home page for the
  <a href="http://www.railstutorial.org/">Ruby on Rails Tutorial</a>
  sample application.
</p>
```

Hacer algo parecido al ejemplo anterior para que los tests pasen. Hacer página de contacto:

```RHTML
<% provide(:title, "Contact") %>
<h1>Contact</h1>
<p>
  Contact the Ruby on Rails Tutorial about the sample app at the
  <a href="http://www.railstutorial.org/contact">contact page</a>.
</p>
```

Cambiar la página de root y borrar del controlador de la aplicación lo primero que hicimos. Modificar **config/routes.rb:**

```ruby
root 'static_pages#home'
```

[Parte 1.5 - Configuración avanzada de testeo](https://github.com/Elolawyn/Rails5Tutorial/tree/master/docs/01_5/README.md)
