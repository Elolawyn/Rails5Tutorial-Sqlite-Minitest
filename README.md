# Tutorial de Ruby on Rails 5

Aplicación resultante del [Tutorial de Ruby on Rails](http://www.railstutorial.org/) de [Michael Hartl](http://www.michaelhartl.com/).

<div id='index'/>
## Índice

1. [Entorno](#seccion01)
2. [Instalación](#seccion02)
3. [Parte 1 - Comienzo de la aplicación](https://github.com/Elolawyn/Rails5Tutorial/tree/master/docs/01/README.md)
4. [Parte 1.5 - Configuración avanzada de testeo](https://github.com/Elolawyn/Rails5Tutorial/tree/master/docs/01_5/README.md)
5. [Parte 2 - Helpers](https://github.com/Elolawyn/Rails5Tutorial/tree/master/docs/02/README.md)
6. [Parte 3 - Aspecto de la aplicación](https://github.com/Elolawyn/Rails5Tutorial/tree/master/docs/03/README.md)
7. [Parte 4 - Modelando usuarios](https://github.com/Elolawyn/Rails5Tutorial/tree/master/docs/04/README.md)
8. [Parte 5 - Registro de usuarios](https://github.com/Elolawyn/Rails5Tutorial/tree/master/docs/05/README.md)
9. [Parte 6 - Login básico](https://github.com/Elolawyn/Rails5Tutorial/tree/master/docs/06/README.md)
10. [Parte 7 - Login avanzado](https://github.com/Elolawyn/Rails5Tutorial/tree/master/docs/07/README.md)
10. [Parte 8 - Actualización, perfil y borrado](https://github.com/Elolawyn/Rails5Tutorial/tree/master/docs/08/README.md)
10. [Parte 9 - Activación de cuenta](https://github.com/Elolawyn/Rails5Tutorial/tree/master/docs/09/README.md)
10. [Parte 10 - Reseteo de contraseña](https://github.com/Elolawyn/Rails5Tutorial/tree/master/docs/10/README.md)
10. [Parte 11 - Microposts](https://github.com/Elolawyn/Rails5Tutorial/tree/master/docs/11/README.md)
10. [Parte 12 - Seguimiento de usuarios](https://github.com/Elolawyn/Rails5Tutorial/tree/master/docs/12/README.md)

<div id='seccion01'/>
## Entorno

[Volver al índice](#index)

1. Ruby 2.3.3
2. Ruby on Rails 5.0.1
3. Ubuntu 16.04.1 LTS
4. SQLite3
5. Minitest

<div id='seccion02'/>
## Instalación

[Volver al índice](#index)

Para instalar ruby se utilizará **rbenv** que permitirá gestionar distintas versiones de ruby y utilizar la más conveniente.

```bash
sudo apt-get update
sudo apt-get install git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev python-software-properties libffi-dev
cd
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
exec $SHELL
git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build
echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.bashrc
exec $SHELL
rbenv install 2.3.3
rbenv global 2.3.3
ruby -v
```

Luego ejecutamos:

```bash
gem install bundler
rbenv rehash
gem install rails -v 5.0.1
rbenv rehash
```

