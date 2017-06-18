# 4.Member_sign 使用者機制
這次要應用的是RUBY的會員登入機制，其中應用到了些實用的Gem(Devise、)，來達到會員機制的實踐，下面會再一一介紹
  
本章作業主題為：登入分成教師與學生，教師可以進行CRUD，學生只能查詢資料

- [系統功能](#系統功能頁面)
- [MODEL設計](#model設計)
- [技術運用(Gem)](#技術運用gem)
- [實作](#開始實作)
  - [Step.1 建立專案](#step1-建立my_store專案)
  - [Step.2 加入gem套件](#step2-加入gem套件)
  - [Step.3 scaffold建立CRUD](#step3-scaffold建立product和coupon的crud)
  - [Step.4 表單驗證](#step4-表單驗證)
 

## 系統功能(頁面)
 
 
- 登入畫面(user/sign_up)

- 首頁(pages)



## MODEL設計

Devise長出來的`User`資料表，我們為了要進行權限管理，還要加上一欄`role`角色欄位，等等我們會用一個`migration`來新增





 

 
## 技術運用(Gem)
  - bootstrap
  - simple_form
  - devise
  - cancancan
> 這個練習會用到devise這套件，可以快速的長出登入畫面，非常的方便
> 這個練習會用到cancancan這套件，對於判斷登入狀態非常的方便，方便我們等等進行角色權限管理

## 開始實作~

### Step.1 建立school專案
  `rails new school`
  
### Step.2 加入gem套件

**[Bootstrap-sass](https://github.com/momo200e/Ruby_Rails_Notes/blob/master/Gem_Notes.md#bootstrap-sass) && [Simple_form](https://github.com/momo200e/Ruby_Rails_Notes/blob/master/Gem_Notes.md#simple_form)**&&**[Devise詳細應用請查閱我另外整理的資料](https://github.com/momo200e/Ruby_Rails_Notes/blob/master/Gem_Notes.md#devise會員機制)**
`cd my_store`進入專案目錄後
```ruby
#Gemfile
gem 'bootstrap-sass'
gem 'simple_form'
gem 'devise'
``` 
終端機中執行`bundle install`

```ruby
#application.scss
@import "bootstrap-sprockets";
@import "bootstrap";
#application.js
//= require bootstrap
``` 
因為要搭配bootstrap使用，所以使用`rails generate simple_form:install --bootstrap`
    
再來是安裝Devise
`rails g devise:install`
  
接著用`rails g devise user`，建立user使用者model和登入頁面...等，聰明的Devise已經把相關設定做好了~ 真方便~
  
記得執行`rails db:migrate`，再重啟server
  
這樣就完成Devise的導入囉~
  
**路徑在：URL/users/sign_up**
  
**[Devise詳細應用請查閱我另外整理的資料](https://github.com/momo200e/Ruby_Rails_Notes/blob/master/Gem_Notes.md#devise會員機制)**


### Step.3 加入首頁
目前我們有了登入及註冊，缺登入後的首頁~~~~~~~~~~~~~~~~~~~~~~~
```ruby
#routes.rb
root "page#index"
```

**新增一個page頁面的當首頁**

`rails g controller pages`快速建立controller

```ruby
#pages_controller.rb
root "page#index"
```
**接著建立一個view，判斷是否為登入狀態**
```ruby
#index.html.erb
<% if user_signed_in? %>
  您好，<%= current_user.email %>
  <%= link_to "登出", destroy_user_session_path, method: "delete" %>
<% else  %>
  <%= link_to "註冊", new_user_registration_path , method: "get" %>
  <%= link_to "登入", new_user_session_path , method: "get" %>
<% end %>
```
> Devise有提供許多實用的方法，其中`user_signed_in?`判斷登入狀態
> `current_user.X`可以取得User資料表裡的欄位


### Step.4 cancancan 實作角色權限管理 - Part 1
**為了要判斷登入身分，所以要用`migration`先新增身分欄位role**
新增一個`migration`  `rails g migration add_role`
```ruby
# migrate
 def change
    add_column :user, :role, :string, default: "student"
 end
```
然後記得`rails db:migrate`

接下來就可以使用cancancan的gem套件羅~~~~

### Step.5 cancancan 實作角色權限管理 - Part 2
cancancan是一個角色管理權限的套件，

當你在做一個很大的案子時，有許多角色權限

如果一樣在每個view和controller都要加上限制，

那真的是煩死人的事情，萬一有哪裡忘記加上權限管理，

你沒有辦法一目了然地找到。

所以，有沒有什麼辦法把所有的權限管理給集中起來，再也不分view和controller呢？

這個時候你就會用到cancancan這個套件了，
Gemfile加入`gem 'cancancan', '~> 1.10'`後，`bundle install`
接著執行`rails g cancan:ability`，你就會得到一支`aibility.rb`檔案了

**全部的權限都可以放在裡面太方便啦~~~~ OAO**

```ruby
# aibility.rb
user ||= User.new
if user.staff?
 can :manage, :all
else
 can :read, :all
end
```
其中`can :manage, :all`意思是此使用者，擁有所有資料的變更權限

`can :read, :all`意思是此使用者，只能讀資料

**[cancancan詳細應用請查閱我另外整理的文章](https://github.com/momo200e/Ruby_Rails_Notes/blob/master/Gem_Notes.md#cancancan)**

這邊用一個`staff?`方法，判斷登入的帳號角色是否為staff
這方法建議在modle上新增
```ruby
# User.rb
def staff?
 self.role == "staff"
end
```
首頁顯示設定

```ruby
#index.html.erb
<% if can? :create, :item %>
您是管理員唷~~~
<% end %>
```
這樣就大功告成囉~~~~
