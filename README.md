# README

## ＜ユーザー系のテーブル＞

## users（ユーザーテーブル）
|Column|Type|Options|Note|
|------|----|-------|----|
|nickname|string|null: false|ニックネーム|
|email|string|null: false, unique: true|メールアドレス|
|password|string|null: false|パスワード（6〜128文字）|
|introduction|text|-------|自己紹介|
|uid|string|null: false|SNS認証用項目|
|provider|string|null: false|SNS認証用項目|

※SNS認証の参考URL：
https://qiita.com/kazuooooo/items/47e7d426cbb33355590e

### Association
- has_one :user_detail
- has_one :delivery
- has_one :card
- has_many :products
- has_many :comments
- has_many :likes
- has_many :purchases
- has_many :rates


## user_details（ユーザー詳細テーブル）
|Column|Type|Options|Note|
|------|----|-------|----|
|sei|string|null: false|姓|
|mei|string|null: false|名|
|kana_sei|string|null: false|カナ姓|
|kana_mei|string|null: false|カナ名|
|birth|date|null: false|生年月日|
|auth_tel|integer|null: false|認証用携帯番号|
|zip_code|integer|-------|郵便番号|
|todofuken|string|-------|都道府県|
|shikutyoson|string|-------|市区町村|
|banchi|string|-------|番地|
|tatemono|string|-------|建物|
|image|string|-------|プロフィール画像|
|users_id|references|null: false,foreign_key: true|ユーザーID／users.id|

### Association
- belongs_to :user


## deliveries（発送元・届け先テーブル）
|Column|Type|Options|Note|
|------|----|-------|----|
|sei|string|null: false|姓|
|mei|string|null: false|名|
|kana_sei|string|null: false|カナ姓|
|kana_mei|string|null: false|カナ名|
|zip_code|integer|null: false|郵便番号|
|todofuken|string|null: false|都道府県|
|shikutyoson|string|null: false|市区町村|
|banchi|string|null: false|番地|
|tatemono|string|null: false|建物|
|tel|integer|null: false|電話番号|
|users_id|references|null: false,foreign_key: true|ユーザーID／users.id|

### Association
- belongs_to :user
- has_many :purchases


## cards（クレジットカードテーブル）
|Column|Type|Options|Note|
|------|----|-------|----|
|card_number|integer|null: false|カード番号|
|expiration_month|integer|null: false|有効月|
|expiration_year|integer|null: false|有効年|
|security_code|integer|null: false|セキュリティコード|
|users_id|references|null: false,foreign_key: true|ユーザーID／users.id|

### Association
- belongs_to :user
- has_many :purchases


## prefectures（都道府県テーブル）
|Column|Type|Options|Note|
|------|----|-------|----|
|name|string|null: false|都道府県名|
|is_hidden|integer|-------|非表示フラグ|

### Association
なし
- has_many :products


## ＜商品系のテーブル＞

## products（商品テーブル）
|Column|Type|Options|Note|
|------|----|-------|----|
|name|string|null: false|商品名（〜40文字）|
|description|text|null: false|商品説明（〜1000文字）|
|categories_id|references|null: false,foreign_key: true|カテゴリー／categories.id|
|conditions_id|references|null: false,foreign_key: true|商品の状態／conditions.id|
|delivery_fee_pays_id|references|null: false,foreign_key: true|配送料の負担／delivery_fee_pays.id|
|delivery_methods_id|references|null: false,foreign_key: true|配送の方法／delivery_methods.id|
|prefectures_id|references|null: false,foreign_key: true|発送元の地域／prefectures.id|
|shipment_periods_id|references|null: false,foreign_key: true|発送までの日数／shipment_periods.id|
|price|integer|null: false|価格|
|status|string|null: false|出品ステータス（出品停止中・出品中・取引中・売却済み）|
|sizes_id|references|null: false,foreign_key: true|サイズ／sizes.id／categories.size_kinds_idがnullでなければ必須|
|brand|string|null: false|ブランド名／is_brand_presenceがnullでなければ必須|
|users_id|references|null: false,foreign_key: true|ユーザーID／users.id|

### Association
- belongs_to :category
- belongs_to :condition
- belongs_to :delivery_fee_pay
- belongs_to :delivery_method
- belongs_to :prefecture
- belongs_to :shipment_period
- belongs_to :size
- belongs_to :user
- has_many :images
- has_many :comments
- has_many :likes
- has_many :purchases


## large_categories（大カテゴリーテーブル）
|Column|Type|Options|Note|
|------|----|-------|----|
|name|string|null: false|大カテゴリー名|
|sort_by|integer|null: false|並び順|

### Association
- has_many :middle_categories


## middle_categories（中カテゴリーテーブル）
|Column|Type|Options|Note|
|------|----|-------|----|
|name|string|null: false|中カテゴリー名|
|sort_by|integer|null: false|並び順|
|large_categories_id|references|null: false,foreign_key: true|大カテゴリ／large_categories.id|

### Association
- belongs_to :large_category
- has_many :categories


## categories（カテゴリーテーブル）
|Column|Type|Options|Note|
|------|----|-------|----|
|name|string|null: false|カテゴリー名
|sort_by|integer|null: false|並び順|
|size_kinds_id|integer|-------|サイズ種別|
|is_brand_presence|integer|-------|ブランド有無|
|middle_categories_id|references|null: false,foreign_key: true|中カテゴリ|

### Association
- belongs_to :middle_category
- has_many :products


## sizes（サイズテーブル）
|Column|Type|Options|Note|
|------|----|-------|----|
|name|string|null: false|サイズ名|
|sort_by|integer|null: false|並び順|
|size_kinds_id|integer|-------|サイズ種別|

### Association
- has_many :products
- belongs_to :size_kind


## size_kinds（サイズ種別テーブル）
|Column|Type|Options|Note|
|------|----|-------|----|
|name|string|null: false|サイズ種別名（服・靴・キッズ服小・キッズ服大・キッズ靴）|

### Association
- has_many :sizes


## conditions（商品の状態テーブル）
|Column|Type|Options|Note|
|------|----|-------|----|
|name|string|null: false|状態名|
|sort_by|integer|null: false|並び順|

### Association
- has_many :products


## delivery_fee_pays（配送料の負担テーブル）
|Column|Type|Options|Note|
|------|----|-------|----|
|name|string|null: false|配送料の負担名|
|sort_by|integer|null: false|並び順|

### Association
- has_many :products


## delivery_methods（配送の方法テーブル）
|Column|Type|Options|Note|
|------|----|-------|----|
|name|string|null: false|配送の方法名|
|sort_by|integer|null: false|並び順|

### Association
- has_many :products


## shipment_periods（発送までの日数テーブル）
|Column|Type|Options|Note|
|------|----|-------|----|
|name|string|null: false|発送までの日数名|
|sort_by|integer|null: false|並び順|

### Association
- has_many :products


## images（商品画像テーブル）
|Column|Type|Options|Note|
|------|----|-------|----|
|image|string|null: false|商品画像|
|products_id|references|null: false,foreign_key: true|商品ID／products.id|

### Association
- belongs_to :product


## comments（商品コメントテーブル）
|Column|Type|Options|Note|
|------|----|-------|----|
|comment|text|null: false|商品コメント|
|products_id|references|null: false,foreign_key: true|商品ID／products.id|
|users_id|references|null: false,foreign_key: true|ユーザーID／users.id|

### Association
- belongs_to :product
- belongs_to :user


## likes（いいねテーブル）
|Column|Type|Options|Note|
|------|----|-------|----|
|products_id|references|null: false,foreign_key: true|商品ID／products.id|
|users_id|references|null: false,foreign_key: true|ユーザーID／users.id|

### Association
- belongs_to :product
- belongs_to :user


## purchases（商品購入テーブル）
|Column|Type|Options|Note|
|------|----|-------|----|
|payment|integer|null: false|支払い金額|
|deliveries_id|references|null: false,foreign_key: true|届け先ID／deliveries.id|
|cards_id|references|null: false,foreign_key: true|クレジットカードID／cards.id|
|products_id|references|null: false,foreign_key: true|商品ID／products.id|
|users_id|references|null: false,foreign_key: true|購入ユーザーID／users.id|

### Association
- belongs_to :delivery
- belongs_to :card
- belongs_to :product
- belongs_to :user
- has_many :rates


## rates（評価テーブル）
|Column|Type|Options|Note|
|------|----|-------|----|
|rating_type|string|null: false|評価区分（受取評価・購入者評価）|
|scores_id|references|null: false,foreign_key: true|評点ID／scores.id|
|comment|text|null: false|評価コメント|
|purchases_id|references|null: false,foreign_key: true|商品購入ID／purchases.id|
|users_id|references|null: false,foreign_key: true|被評価ユーザーID／users.id|

### Association
- belongs_to :score
- belongs_to :purchase
- belongs_to :user


## scores（評点テーブル）
|name|string|null: false|評点名（良い・普通・悪い）|

### Association
- has_many :rates
