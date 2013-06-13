fake-factory
============

## Usage
### simple

```ruby
FakeFactory(:name)
FakeFactory(:address)
FakeFactory(:username, length: 3..8)
FakeFactory(:sentence, lang: 'en-GB')
```

### instance
```ruby
fake_user = FakeFactory::Person.new
fake_user.name
fake_user.name_kana
fake_user.name_alphabetical
```

### FactoryGirl
```ruby
FactoryGirl.define do
  factory :user do
    fake(:username)
    fake(:password)
    fake(:family_name)
    fake(:given_name)
    fake(:email)
    fake(:nickname, type: :word, lang: :en)
  end
end
```
```ruby
FactoryGirl.define do
  factory :user do
    ignore do
      fake_person { FakeFactory::Person.new }
      fake_location { FakeFactory::Location.new }
    end
    username { fake_person.given_name_alphabetical }
    fake(:password)
    fake(:family_name, of: fake_person)
    fake(:given_name, of: fake_person)
    fake(:family_name_kana, of: fake_person)
    fake(:given_name_kana, of: fake_person)
    fake(:email)
  end
end
```

## Policy
飽くまで Fake なので、基本的に現実の地名の一覧などを持たない。
(ただし、都道府県や米国の州など、おおむね100以下の要素数の辞書は持ってもいいかも)

example domain を適切に使う。

辞書は yml で持つ。ロジックは rb で実装。 (Faker はこの点で混乱している)

FakeFactory::Ja というような形で国際化対応 (Faker はこの点でも混乱している)
FakeFactory::Ja に実装されていないメソッドは FakeFactory::En に fallback する。

FakeFactory(:name) と呼び出した場合はデフォルトの lang が使用されるが、
FakeFactory::En(:name) のように言語を指定して使用することも出来る。
