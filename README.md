# 4.member_sign
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
 
 
- 登入畫面(product)

  書本CRUD



## MODEL設計

我們在建立MODEL的時候，Coupon我們有設一個`allow_multiple_use`欄位，決定該折價卷是否可重複使用，所以我們Product需要欄位`coupon_id`來紀錄折價卷，以方便後續判斷。

**(`coupon_id`欄位之後再用`add_columns`建立)**



**Product 商品資料表**

 - title: String   
 - price: integer   
 - description: text
 - coupon_id: integer

 - 關係：belongs_to :coupon


 
**其中要注意的是，一張折價卷可用在多本不同本書上，是一對多的關係，所以 `has_many :products`和`belongs_to :coupon`建好Model記得加上去**

 
## 技術運用(Gem)
  - bootstrap
  - simple_form
  - devise
> 這個練習會用到devise這套件，對於判斷登入狀態非常的方便


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
  
**[Devise詳細應用請查閱我另外整理的資料](https://github.com/momo200e/Ruby_Rails_Notes/blob/master/Gem_Notes.md#devise會員機制)**



