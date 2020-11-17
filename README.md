# Rails Tutorial Log





## 0. 環境構築

```bash
$ sudo docker-compose build
$ sudo docker-compose run --rm web bundle install
$ sudo docker-compose run --rm web rails db:create
$ docker-compose up
```



## 1.1 環境構築



## 1.2 



## 2.1 Toy_appの作成

ユーザー設計

```bash
$ sudo docker-compose run --rm web rails generate scaffold User name:string email:string
$ sudo docker-compose run --rm web rails db:migrate
```



表示ができなかったが以下のコードを削除すれば直った。

`/app/views/layouts/application.html.erb`

```html
<%= javascript_pack_tag 'application', 'data-turbolinks-track': 'reload' %>
```



練習が全くわからなかったので，4章にいく。

`app/models/user.rb`

```ruby
class User < ApplicationRecord
  has_many :microposts
  validates FILL_IN, presence: true    # 「FILL_IN」をコードに置き換えてください
  validates FILL_IN, presence: true    # 「FILL_IN」をコードに置き換えてください
end
```



## sample_appの作成



### リポジトリを作成

GitHub.comにアクセスして`sample_app`のリポジトリを作成する。



### GitHubからrails-tutorialをclone

`Terminal`

```zsh
$ cd ~/workspace
$ git clone git@github.com:NUTFes/rails-tutorial.git workspace/rails-tutorial
Cloning into 'workspace/rails-tutorial'...
remote: Enumerating objects: 93, done.
remote: Counting objects: 100% (93/93), done.
remote: Compressing objects: 100% (74/74), done.
remote: Total 93 (delta 2), reused 93 (delta 2), pack-reused 0
Receiving objects: 100% (93/93), 22.75 KiB | 1.14 MiB/s, done.
Resolving deltas: 100% (2/2), done.
$ cd ~/workspace/rails-tutorial
$ git remote -v
origin git@github.com:NUTFes/rails-tutorial.git (fetch)
origin git@github.com:NUTFes/rails-tutorial.git (push)
```



### リネーム，自分のリモートリポジトリに変更

`Terminal`

```zsh
$ cd ~/workspace
$ mv ./rails-tutrial ./sample_app
$ cd ~/workspace/sample_app
$ git remote set-url origin git@github.com:TsubasaOura/sample_app.git
$ git remote -v
origin git@github.com:TsubasaOura/sample.git (fetch)
origin git@github.com:TsubasaOura/sample_app.git (push)
$ git add -A
$ git commit -m "First commit"
$ git push
```



### docker-composeで環境構築

`Terminal`

```zsh
$ cd ~/workspace/sample_app
$ dc build
$ dcrw bundle install
$ dcrw rails db:create
$ dc up
```







##  3. 静的なページを作成



### 静的ページのブランチを作成

`Terminal`

```zsh
$ git checkout -b "static-pages"
```



### Viewを制御するControllerを作成

`Terminal`

```zsh
$ dcrw rails g controller StaticPages home help
```



### ルートを作成

`config/route.rb`　

```ruby
get 'static_pages/home'
get 'static_pages/help'
```



### Controllerに追記

`app/controller/static_pages_controller.rb` （静的なページのため変数・関数はなし）

```ruby
class StaticPagesController < ApplicationController
	def home
	end
	def help
	end
	def about
	end
end
```



### ページを作成

`app/views/static_pages/home.html.erb`

```html
<h1>Sample App</h1>
<p>
	This is the home page for the
	<a href="https://railstutorial.jp/">Ruby on RailsTutorial</a>
	sample application.
</p>
```

`app/views/static_pages/help.html.erb`

```html
<h1>Help</h1>
<p>
	Get help on the Ruby on Rails Tutorial at the
	<a href="https://railstutorial.jp/help">Rails Tutorial help page</a>.
	To get help on this sample app, see the
	<a href="https://railstutorial.jp/#ebook"><em>Ruby on Rails Tutorial</em> book</a>.
</p>
```



### 練習 ~aboutのページを作ってみる~

`app/views/static_pages/about.html.erb`　を作成

```html
<h1>About</h1>
<p>
	<a href="https://railstutorial.jp/">Ruby on Rails Tutorial</a>
    is a 
    <a href="https://railstutorial.jp/#ebook">book</a> 
	and
	<a href="https://railstutorial.jp/#screencast">screencast</a> 
	to teach web development with
 	<a href="http://rubyonrails.org/">Ruby on Rails</a>.
 	This is the sample application for the tutorial.
</p>
```

`config/route.rb`　ルーティング設定

```ruby
get 'static_pages/home'
get 'static_pages/help'
get 'static_pages/about'
```



### HTMLの構造を整える

* `!DOCTYPE`，`provide`，`head`，`body`　を追記
* `yeld(:title)`　をつかってタイトルを呼び出す

`app/views/static_pages/home.html.erb`

```html
<% provide(:title,"Home") %>
<!DOCTYPE html>
<html>
    <head>
        <title><%= yield(:title) %> | Ruby on Rails Tutorial Sample App</title>
    </head>
    <body>
 		<h1>Sample App</h1>
 		<p>
			This is the home page for the
 			<a href="https://railstutorial.jp/">Ruby on Rails Tutorial</a>
 			sample application.
 		</p>
 	</body>
</html>
```

`app/views/static_pages/help.html.erb`

```html
<% provide(:title,"Help") %>
<!DOCTYPE html>
<html>
    <head>
        <title><%= yield(:title) %> | Ruby on Rails Tutorial Sample App</title>
    </head>
    <body>
        <h1>Help</h1>
        <p>
            Get help on the Ruby on Rails Tutorial at the 
            <a href = " https://railstutorial.jp/help">Rails Tutorial help page</a>. 
            To get help on this sample app, see the 
            <a href = "https://railstutorial.jp/#ebook">Ruby on Rails Tutorial book</a>.
        </p>
    </body>
</html>
```

`app/views/static_pages/about.html.erb`

```html
<% provide(:title, "About") %>
<!DOCTYPE html>
<html>
    <head>     
    </head>
    <body>
        <h1>About</h1>
        <p>
            <a href="https://railstutorial.jp/">Ruby on Rails Tutorial</a>
            is a <a href="https://railstutorial.jp/#ebook">book</a>and
            <a href="https://railstutorial.jp#screencast">screencast</a>
            to teach web development with
            <a href="http://rubyonrails.org/">Ruby on Rails</a>.
            This is the sample application for the tutorial.
        </p>
    </body>
</html>
```



### 演習 ~contactページを作ってみる~

`app/views/static_pages/contact.html.erb`

```html
<% provide(:title, "Contact") %>
    <h1>Contact</h1>
    <p>
        Contact the Ruby on Rails Tutorial about the sample app at the
        <a href="https://railstutorial.jp/contact">contact page</a>.
    </p>
```

`config/rutes.rb`を編集

```ruby
Rails.application.routes.draw do
  get 'static_pages/home'
  get 'static_pages/help'
  get 'static_pages/about'
  get 'static_pages/contact'
end
```



### Gitにあげる

`Terminal`

```zsh
$ git add -A
$ git commit -m "Finish static pages"
$ git push origin static-pages
```

マスターにマージする

`Terminal`

```zsh
$ git check out master
$ git merge static-pages
$ git push
```



## 4. とばす



## 5. レイアウトを作成



### レイアウトのブランチを作成

`Terminal`

```zsh
$ git checkout -b "filling-in-layout"
```



### アプリケーションのページを作成

`app/views/layouts/application.html.erb`

```html
<!DOCTYPE html>
<html>
 	<head>
        <title><%= full_title(yield(:title)) %></title>
        <%= csrf_meta_tags %>
        <%= stylesheet_link_tag 'application', media: 'all','data-turbolinks-track':'reload' %>
        <!--[if lt IE 9]>
            <script src="//cdnjs.cloudflare.com/ajax/libs/html5shiv/r29/html5.min.js">
            </script>
        <![endif]-->
 	</head>
 	<body>
 		<header class="navbar navbar-fixed-top navbar-inverse">
        <div class="container">
            <%= link_to "sample app", '#', id: "logo" %>
            <nav>
                <ul class="nav navbar-nav navbar-right">
                    <li><%= link_to "Home", '#' %></li>
                    <li><%= link_to "Help", '#' %></li>
                    <li><%= link_to "Log in", '#' %></li>
                </ul>
            </nav>
        </div>
    </header>
        <div class="container">
        <%= yield %>
        </div>
    </body>
</html>
```



### Homeを編集

`app/views/static_pages/home.html.erb`

```html
<% provide(:title,"Home") %>
<!DOCTYPE html>
<html>
    <head>
        <title><%= yield(:title) %> | Ruby on Rails Tutorial Sample App</title>
    </head>
    <body>
        <div class="center jumbotron">
            <h1>Welcome to the Sample App</h1>
            
            <h2>
                This is the home page for the 
                <a href = " https://railstutorial.jp">Ruby on Rails Tutorial</a> 
                sample application.
            </h2>

            <%= link_to "Sign up now!", '#', class: "btn btn-lg btn-primary" %>
        </div>

        <%= link_to image_tag("rails.png", alt: "Rails logo"), 'http://rubyonrails.org' %>
    </body>
</html>
```

コマンドを使ってLogoをダウンロード

`Terminal`

```zsh
$ curl -o app/assets/images/rails.ping -OL railstutorial.jp/rails.ping
```



### Bootstrapをインストール

`gemfile`

```ruby
.
.
.
gem 'bootstrap-sass', '3.3.7'
```

`Terminal`

```zsh
$ dcrw bundle install
```



### 問題点

bootstrapをインストール後`docker-compose up`と`rails s`ができなくなる。

```zsh
Could not find execjs-2.7.0 in any of the sources
```

`docker-compose build`したら直る

`Terminal`

```zsh
$ dc build
```



### CSSを作成

`app/assets/stylesheets/custom.scss`

```css
@import "bootstrap-sprockets";
@import "bootstrap";

/* universal */

body {
    padding-top: 60px;
}

section {
    overflow: auto;
}

textarea {
    resize: vertical;
}

.center {
    text-align: center;
}

.center h1 {
    margin-bottom: 10px;
}

/* typography */

h1, h2, h3, h4, h5,h6 {
    line-height: 1;
}

h1 {
    font-size: 3em;
    letter-spacing: -2px;
    margin-bottom: 30px;
    text-align: center;
}

h2 {
    font-size: 1.2em;
    letter-spacing: -1px;
    margin-bottom: 30px;
    text-align: center;
    font-weight: normal;
    color: #777;
}

p {
    font-size: 1.1em;
    line-height: 1.7em;
}

/* header */

#logo {
    float: left;
    margin-right: 10px;
    font-size: 1.7em;
    color: #fff;
    text-transform: uppercase;
    padding-top: 9px;
    font-weight: bold;
}

#logo:hover {
    color: #fff;
    text-decoration: none;
}

/* footer */

footer {
    margin-top: 45px;
    padding-top: 5px;
    border-top: 1px solid #eaeaea;
    color: #777;
}

footer a {
    color: #555;
}

footer a:hover {
    color: #222;
}

footer small {
    float: left;
}

footer ul {
    float: right;
    list-style: none;
}

footer ul li {
    float: left;
    margin-left: 15px;
}
```



### パーシャル化

よくつかうヘッダー・フッター・スクリプトをパーシャル化

`app/views/layouts/application.html.erb`

```html
<!DOCTYPE html>
<html>
  <head>
    <title><%= yield(:title) %> | Ruby on Rails Tutorial Sample App</title>
    <%= csrf_meta_tags %>
    <%= csp_meta_tag %>

    <%= stylesheet_link_tag 'application', media: 'all', 'data-turbolinks-track': 'reload' %>

    <%= render 'layouts/shim' %>
  </head>
  <body>
    <%= render 'layouts/header' %>
    <div class="container">
      <%= yield %>
    </div>
    <%= render 'layouts/footer' %>
  </body>
</html>
```

`app/views/layouts/_shim.html.erb`

```html
<!--[if lt IE 9]>
  <script src="//cdnjs.cloudflare.com/ajax/libs/html5shiv/r29/html5.min.js">
  </script>
<![endif]-->
```

`app/views/layouts/_header.html.erb`

```html
<header class="navbar navbar-fixed-top navbar-inverse"> 
    <div class="container">
    <%= link_to"sample app", root_path, id: "logo" %>
    <nav>
        <ul class="nav navbar-nav navbar-right">
        <li><%= link_to "Home", root_path %></li>
        <li><%= link_to "Help", help_path %></li>
        <li><%= link_to "Log in", contact_path %></li>
        </ul>
    </nav>
    </div>
</header>
```

`app/views/layouts/_footer.html.erb`

```html
<footer class="footer"> 
    <small>
        The <a href="https://railstutorial.jp/">Ruby on Rails Tutorial</a>
        by <a href="http://www.michaelhartl.com/">Michael Hartl</a>
    </small>
    <nav>
        <ul>
        <li><%= link_to "About", about_path %></li>
        <li><%= link_to "Contact", contact_path %></li>
        <li><a href="http://news.railstutorial.org/">News</a></li>
        </ul>
    </nav>
</footer>
```



### レイアウトのリンクを設定

`config/routes.rb`

```ruby
Rails.application.routes.draw do
  root 'static_pages#home'
  get '/help', to: 'static_pages#help'
  get '/about', to: 'static_pages#about'
  get '/contact', to: 'static_pages#contact'
end
```

`app/views/layouts/_header.html.erb`

```html

<%= link_to"sample app", root_path, id: "logo" %>

<li><%= link_to "Home", root_path %></li>
<li><%= link_to "Help", help_path %></li>
<li><%= link_to "Log in", contact_path %></li>

```

`app/views/layouts/_footer.html.erb`

```html

<li><%= link_to "About", about_path %></li>
<li><%= link_to "Contact", contact_path %></li>

```



### ユーザー登録ページ

`config/routes.rb`

```ruby
Rails.application.routes.draw do
    root 'static_pages#home'
    get '/help', to: 'static_pages#help'
    get '/about', to: 'static_pages#about'
    get '/contact', to: 'static_pages#contact'
    get '/signup', to: 'users#new'
end
```

`app/views/static_pages/home.html.erb`

```html
<%= link_to "Sign up now!", signup_path, class: "btn btn-lg btnprimary" %> 
```

`app/views/users/new.html.erb`

```html
<% provide(:title, 'Sign up') %>
<h1>Sign up</h1>
<p>This will be a signup page for new users.</p>
```

 

### Gitにあげる

`Terminal`

```zsh
$ git add -A
$ git commit -m "Finish layout routes"
$ git push origin filling-in-layout
```

masterにマージ

`Terminal`

```zsh
$ git checkout master
$ git merge filling-in-laiout
$ git push
```



## 6. ユーザーモデルの作成



### ユーザーモデルのブランチを作成

`Terminal`

```zsh
$ git checkout -b "modeling-users"
```



### ユーザーモデルを作成

`Terminal`

```zsh
$ dcrw rails g model User name:string email:string
$ dcrw rails db:migrate
```

`db/migrate/[timestamp]_create_users.rb`

```ruby
class CreateUsers < ActiveRecord::Migration[5.0]
    def change
        create_table :users do |t|
            t.string :name
            t.string :email
            t.timestamps
        end
    end
end
```



### ユーザーモデルを編集

* `presence true`  で要入力に
* `length`  で入力の長さを制限

```ruby
class User < ApplicationRecord
    validates :name, presence: true, length:{maximum:50} 
    validates :email, presence: true, length:{maximum:255}
end
```



### Consoleでオブジェクトを作成

`Terminal`

```zsh
$ dcrw rails console --sandbox
```

`rails console`

```console
>> User.new

>> user = User.new(name: "Michael Hartl", email: "mhartl@example.com")

>> user.valid?
=> true

>> user.save

>> user.name
=> "Michael Hartl"

>> user.email
=> "mhartl@example.com"

>> user.updated_at
=> Mon, 23 May 2016 19:05:58 UTC +00:00

>> User.create(name: "A Nother”, email:"another@example.org")

>> foo = User.create(name: "Foo", email: "foo@bar.com")

>> foo.destroy

>> foo

>> User.find(1)

>> User.find(3)

>> User.find_by(email: "mhartl@example.com")

>> User.all
```



### データに規則をつける

`app/models/user.rb`

```ruby
class User < ApplicationRecord
    before_save { self.email = email.downcase }
    validates :name, presence: true, length: { maximum: 50 }
    VALID_EMAIL_REGEX = /\A[\w+\-.]+@[a-z\d\-]+(\.[a-z\d\-]+)*\.[a-z]+\z/i
    validates :email, presence: true, length: { maximum: 255 }, format: { with: VALID_EMAIL_REGEX }, uniqueness: { case_sensitive: false} 
end
```

`Terminal`

```zsh
$ dcrw rails generate migration add_index_to_users_email
```



### パスワード付きのユーザーモデルを作成

`Terminal`

```zsh
$ dcrw rails generate migration add_password_digest_to_users password_digest:string
$ dcrw rails db:migragte
```



### パスワードをハッシュ化するgemをインストール

`Gemfile`

```ruby
.
.
.
gem 'bcrypt', '3.1.12' 
```

`Terminal`

```zsh
$ dcrw Bundle install
```



### パスワードのデータ規則

`app/models/user.rb`

```ruby
class User < ApplicationRecord
    before_save { self.email = email.downcase }
    validates :name, presence: true, length: { maximum: 50 }
    VALID_EMAIL_REGEX = /\A[\w+\-.]+@[a-z\d\-]+(\.[a-z\d\-]+)*\.[a-z]+\z/i
    validates :email, presence: true, length: { maximum: 255 }, format: { with: VALID_EMAIL_REGEX},uniqueness: {case_sensitive: false}
    has_secure_password, presence:true, length:{ minimum: 6 }
end
```



### Consoleで確認

`Terminal`

```zsh
$ dcrw rails c
```

`console`

```zsh
>> User.create(name: "Michael Hartl", email: "mhartl@example.com", password: "foobar", password_confirmation: "foobar") 

>> user = User.find_by(email: "mhartl@example.com")

>> user.password_digest
=> "$2a$10$xxucoRlMp06RLJSfWpZ8hO8Dt9AZXlGRi3usP3njQg3yOcVFzb6oK"

>> user.authenticate("not_the_right_password")
false
>> user.authenticate("foobaz")
false

>> user.authenticate("foobar")
=> #<User id: 1, name: "Michael Hartl", email: "mhartl@example.com",
created_at: "2016-05-23 20:36:46", updated_at: "2016-05-23 20:36:46",
password_digest: "$2a$10$xxucoRlMp06RLJSfWpZ8hO8Dt9AZXlGRi3usP3njQg3...">

>> !!user.authenticate("foobar")
=> true
```



### Gitにあげる

`Terminal`

```zsh
$ git add -A
$ git commit - "Make a basic User model"
$ git push origin modeling-users
```

masterにマージ

`Terminal`

```zsh
$ git checkout master
$ git merge modeling-users
$ git push
```



## 7. ユーザー登録



### ユーザー登録のブランチを作成

`Terminal`

```zsh
$ git chechout -b "sign-up"
```



### デバック機能を作成

`app/view/layouts/application.html.erb`

```html
<body>
    <%= render 'layouts/header' %>
    <div class="container">
        <%= yield %>
        <%= render 'layouts/footer' %>
        <%= debug(params) if Rails.env.development? %>
    </div>
</body> 
```



`app/assets/stylesheets/custom.scss`

```css
@import "bootstrap-sprockets";
@import "bootstrap";
/* mixins, variables, etc. */
$gray-medium-light: #eaeaea;
@mixin box_sizing {
    -moz-box-sizing: border-box;
    -webkit-box-sizing: border-box;
    box-sizing: border-box;
}
.
.
.
/* miscellaneous */
.debug_dump {
    clear: both;
    float: left;
    width: 100%;
    margin-top: 45px;
    @include box_sizing;
}
```



### Userを表示させる

ルート設定

`config/routes.rb`

```ruby
Rails.application.routes.draw do
    root 'static_pages#home'
    get '/help', to: 'static_pages#help'
    get '/about', to: 'static_pages#about'
    get '/contact', to: 'static_pages#contact'
    get '/signup', to: 'users#new'
    resources :users
end
```



`app/controllers/users_controller.rb`

```ruby
class UsersController < ApplicationController
    def show
    @user = User.find(params[:id])
    end
    def new
    end
end
```



`app/views/users/show.html.erb`

```html
<%= @user.name %>, <%= @user.email %>
```



### User登録画面を作成



`app/controllers/users_controller.rb`

```ruby
class UsersController < ApplicationController
    def show
    @user = User.find(params[:id])
    end
    def new
    @user = User.new
    end
end
```



`app/views/users/new.html.erb`

```html
<% provide(:title, 'Sign up') %>
<h1>Sign up</h1>
<div class="row">
    <div class="col-md-6 col-md-offset-3">
        <%= form_for(@user) do |f| %>
            <%= f.label :name %>
            <%= f.text_field :name %>
            <%= f.label :email %>
            <%= f.email_field :email %>
            <%= f.label :password %>
            <%= f.password_field :password %>
            <%= f.label :password_confirmation, "Confirmation" %>
            <%= f.password_field :password_confirmation %>
            <%= f.submit "Create my account", class: "btn btn-primary" %>
        <% end %>
    </div>
</div>
```



`app/assets/stylesheets/custom.scss`

```css
.
.
.
/* forms */
input, textarea, select, .uneditable-input {
    border: 1px solid #bbb;
    width: 100%;
    margin-bottom: 15px;
    @include box_sizing;
}
input {
	height: auto !important;
} 
```



`app/views/users/new.html.erb`

```html
<% provide(:title, 'Sign up') %>
<h1>Sign up</h1>
<div class="row">
    <div class="col-md-6 col-md-offset-3">
        <%= form_for(@user) do |f| %>
            <%= f.label :name %>
            <%= f.text_field :name %>
            <%= f.label :email %>
            <%= f.email_field :email %>
            <%= f.label :password %>
            <%= f.password_field :password %>
            <%= f.label :password_confirmation, "Confirmation" %>
            <%= f.password_field :password_confirmation %>
            <%= f.submit "Create my account", class: "btn btn-primary" %>
        <% end %>
    </div>
</div>
```



`app/assets/stylesheets/custom.scss`

```css
.
.
.
/* forms */
input, textarea, select, .uneditable-input {
    border: 1px solid #bbb;
    width: 100%;
    margin-bottom: 15px;
    @include box_sizing;
}
input {
	height: auto !important;
} 
```



`app/controllers/users_controller.rb`

```ruby
class UsersController < ApplicationController

  def show
    @user = User.find(params[:id])
  end

  def new
    @user = User.new
  end

  def create
    @user = User.new(user_params)
    if @user.save
      flash[:success] = "Welcome to the Sample App!"
      render_to @user
    else
      render 'new'
    end
  end
end
```



`app/views/layouts/application.html.erb`

```html
<!DOCTYPE html>
<html>
 .
 .
 .
<body>
    <%= render 'layouts/header' %>
    <div class="container">
        <% flash.each do |message_type, message| %>
        <%= content_tag(:div, message, class: "alert alert-#{message_type}") %>
        <% end %>
        <%= yield %>
        <%= render 'layouts/footer' %>
        <%= debug(params) if Rails.env.development? %>
    </div>
    .
    .
    .
</body>
</html>
```



`app/views/shared/_error_messages.html.erb`

```html
<% if @user.errors.any? %>
    <div id="error_explanation">
        <div class="alert alert-danger">
            The form contains <%= pluralize(@user.errors.count, "error") %>.
        </div>
        <ul>
            <% @user.errors.full_messages.each do |msg| %>
            <li><%= msg %></li>
            <% end %>
        </ul>
    </div>
<% end %>
```



`app/assets/stylesheets/custom.scss`

```css
.
.
/* forms */
.
.
#error_explanation {
    color: red;
    ul {
        color: red;
        margin: 0 0 30px 0;
    }
}
.field_with_errors {
    @extend .has-error;
    .form-control {
        color: $state-danger-text;
    }
}
```



`app/views/users/new.html.erb`

```html
<% provide(:title, 'Sign up') %>
<h1>Sign up</h1>
<div class="row">
    <div class="col-md-6 col-md-offset-3">
        <%= form_for(@user) do |f| %>
            <%= render 'shared/error_messages' %>
            <%= f.label :name %>
            <%= f.text_field :name %>
            <%= f.label :email %>
            <%= f.email_field :email %>
            <%= f.label :password %>
            <%= f.password_field :password %>
            <%= f.label :password_confirmation, "Confirmation" %>
            <%= f.password_field :password_confirmation %>
            <%= f.submit "Create my account", class: "btn btn-primary" %>
        <% end %>
    </div>
</div>
```



### Gitにあげる

`Terminal`

```zsh
$ git add -A
$ git commit -m "Finish user signup"
$ git push prigin sign-up
```

masterにマージ

`Terminal`

```zsh
$ git checkout master
$ git merge sign-up
$ git push
```



### 8. 基本的なログイン機構

### 9. 発展的なログイン機構







 A server is already running. Check /app_name/tmp/pids/server.pid.

.pid サーバーのログファイル？

ログインしたログが残ってて不具合が起きている

`app/views/shared/_error_messages.html.erb`

```html
<% if @user.errors.any? %>
    <div id="error_explanation">
        <div class="alert alert-danger">
            The form contains <%= pluralize(@user.errors.count, "error") %>.
        </div>
        <ul>
            <% @user.errors.full_messages.each do |msg| %>
            <li><%= msg %></li>
            <% end %>
        </ul>
    </div>
<% end %>
```



`app/assets/stylesheets/custom.scss`

```css
.
.
/* forms */
.
.
#error_explanation {
    color: red;
    ul {
        color: red;
        margin: 0 0 30px 0;
    }
}
.field_with_errors {
    @extend .has-error;
    .form-control {
        color: $state-danger-text;
    }
}
```



`app/views/users/new.html.erb`

```html
<% provide(:title, 'Sign up') %>
<h1>Sign up</h1>
<div class="row">
    <div class="col-md-6 col-md-offset-3">
        <%= form_for(@user) do |f| %>
            <%= render 'shared/error_messages' %>
            <%= f.label :name %>
            <%= f.text_field :name %>
            <%= f.label :email %>
            <%= f.email_field :email %>
            <%= f.label :password %>
            <%= f.password_field :password %>
            <%= f.label :password_confirmation, "Confirmation" %>
            <%= f.password_field :password_confirmation %>
            <%= f.submit "Create my account", class: "btn btn-primary" %>
        <% end %>
    </div>
</div>
```



### Gitにあげる

`Terminal`

```zsh
$ git add -A
$ git commit -m "Finish user signup"
$ git push prigin sign-up
```

masterにマージ

`Terminal`

```zsh
$ git checkout master
$ git merge sign-up
$ git push
```

