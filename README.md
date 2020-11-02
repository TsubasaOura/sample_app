# Rails Tutorial Log (sample_app)

## 第4章

環境構築

```bash
git clone git@github.com:NUTFes/rails-tutorial.git workspace/rails-tutorial
mv ./rails-tutrial ./sample_app
git remote -v
git remote set-url origin git@github.com:TsubasaOura/sample_app.git
git remote -v
git add -A
git commit -m "First commit"
git push
git checkout -b "static-pages"
```



`app/views/static_pages`の

`home.html.erb` `help/html.erb` `about.html.erb` `contact.htm.erb` を編集



`config/rutes.rb`を編集

```ruby
Rails.application.routes.draw do
  root 'static_pages#home'
  get 'static_pages/home'
  get 'static_pages/help'
  get 'static_pages/about'
  get 'static_pages/contact'
end
```



## 第5章



bootstrapをインストール後`docker-compose up`と`rails s`ができなくなる。

```bash
Could not find execjs-2.7.0 in any of the sources
```



