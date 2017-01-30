# Parte 7 - Login avanzado

[Volver al repositorio](https://github.com/Elolawyn/Rails5Tutorial) - [Parte 6 - Login básico](https://github.com/Elolawyn/Rails5Tutorial/tree/master/docs/06/README.md)

rails generate migration add_remember_digest_to_users remember_digest:string
rails db:migrate


  app/models/user.rb
  # Returns a random token.
  def User.new_token
    SecureRandom.urlsafe_base64
  end

  attr_accessor :remember_token

  # Remembers a user in the database for use in persistent sessions.
  def remember
    self.remember_token = User.new_token
    update_attribute(:remember_digest, User.digest(remember_token))
  end
  
  # Returns true if the given token matches the digest.
  def authenticated?(remember_token)
    return false if remember_digest.nil?
    BCrypt::Password.new(remember_digest).is_password?(remember_token)
  end
  
  # Forgets a user.
  def forget
    update_attribute(:remember_digest, nil)
  end
  
  
  app/controllers/sessions_controller.rb
  def create
    user = User.find_by(email: params[:session][:email].downcase)
    if user && user.authenticate(params[:session][:password])
      log_in user
      remember user
      redirect_to user
    else
      flash.now[:danger] = 'Invalid email/password combination'
      render 'new'
    end
  end

  app/helpers/sessions_helper.rb
  # Remembers a user in a persistent session.
  def remember(user)
    user.remember
    cookies.permanent.signed[:user_id] = user.id
    cookies.permanent[:remember_token] = user.remember_token
  end
  
  # Returns the user corresponding to the remember token cookie.
  def current_user
    if (user_id = session[:user_id])
      @current_user ||= User.find_by(id: user_id)
    elsif (user_id = cookies.signed[:user_id])
      user = User.find_by(id: user_id)
      if user && user.authenticated?(cookies[:remember_token])
        log_in user
        @current_user = user
      end
    end
  end
  
  test/integration/users_login_test.rb
  test "login with valid information followed by logout" do
    get login_path
    post login_path, params: { session: { email: @user.email, password: 'password' } }
    assert is_logged_in?
    assert_redirected_to @user
    follow_redirect!
    assert_template 'users/show'
    assert_select "a[href=?]", login_path, count: 0
    assert_select "a[href=?]", logout_path
    assert_select "a[href=?]", user_path(@user)
    delete logout_path
    assert_not is_logged_in?
    assert_redirected_to root_url
    # Simulate a user clicking logout in a second window.
    delete logout_path
    follow_redirect!
    assert_select "a[href=?]", login_path
    assert_select "a[href=?]", logout_path,      count: 0
    assert_select "a[href=?]", user_path(@user), count: 0
  end
  
  app/controllers/sessions_controller.rb
  def destroy
    log_out if logged_in?
    redirect_to root_url
  end
  
  test/models/user_test.rb
    test "authenticated? should return false for a user with nil digest" do
    assert_not @user.authenticated?('')
  end


[Parte 8 - Actualización, perfil y borrado](https://github.com/Elolawyn/Rails5Tutorial/tree/master/docs/08/README.md)
