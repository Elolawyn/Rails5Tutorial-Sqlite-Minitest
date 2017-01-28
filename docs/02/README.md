# Parte 2 - Helpers

[Volver al repositorio](https://github.com/Elolawyn/Rails5Tutorial) - [Parte 1.5 - Configuración avanzada de testeo](https://github.com/Elolawyn/Rails5Tutorial/tree/master/docs/01_5/README.md)

En esta parte vamos a crear el primer helper.

### Mejorar el proceso de testeo:

Modificar **app/helpers/application_helper.rb:**

```ruby
# Returns the full title on a per-page basis.
def full_title(page_title = '')
	base_title = "Ruby on Rails Tutorial Sample App"
	if page_title.empty?
	  base_title
	else
	  page_title + " | " + base_title
	end
end
```

Modificar **app/views/layouts/application.html.erb:**

```RHTML
<title><%= full_title(yield(:title)) %></title>
```

Eliminar de **app/views/static_pages/home.html.erb** el provide. Modificar los tests a la página home en **test/controllers/static_pages_controller_test.rb:**

```ruby
assert_select "title", "Ruby on Rails Tutorial Sample App"
```

[Parte 3 - Aspecto de la aplicación](https://github.com/Elolawyn/Rails5Tutorial/tree/master/docs/03/README.md)
