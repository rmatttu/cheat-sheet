# Ruby on Rails

## Usage


プロジェクトフォルダー作成

```bash
rails _5.2.1_ new <project_name>
cd <project_name>

# 必要ない？
bundle install

git add .
git commit
# First commit
```


モデル作成例

```bash
# User model (id, name, email)
rails generate scaffold User name:string email

# Update database
rails db:migrate
```

コントローラーの作成例

```bash
# /static_pages/home
rails generate controller StaticPages home help
```

ルーティング設定
```bash
vim <project_name>/config/routes.rb
```

デバッグ用サーバ起動

```bash
rails server
```

http://<サーバIP>:3000/ にアクセス


テスト

```bash
rails test
```


## 参考

* https://railstutorial.jp
