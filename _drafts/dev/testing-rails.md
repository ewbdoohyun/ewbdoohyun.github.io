## Model ##

What to test
- The model’s create method, when passed valid attributes, should be valid.
- Data that fail validations should not be valid.
- Class and instance methods perform as expected.

- 유효한 attritutes를 넘겼을 때 model의 create method가 유효해야한다.
    {% highlight text %}
        item = Item.new(name: "ABC")
        expect(item).to be_valid
    {% endhighlight %}
- 유효하지 않은 데이터는 유효성 검사를 통과하지 않아야 한다.
    {% highlight text %}
        item.valid?
        expect(item.errors[:name]).to include("can't be blank")
    {% endhighlight %}
- 클래스 매소드(+scope)와 인스턴스 매소드가 예상한것과 같이 동작한다.
    {% highlight text %}
        item_a = Item.create()
        item_b = Item.create()
        item_c = Item.create(created_at : 1.day.ago)
        expect(Contact.created_today).to eq [item_a, item_b]
    {% endhighlight %}

## FactoryGirl vs. Fixture ##
- Fixture 단점
    - Active Record를 우회 - validation check이 안됨
    - 데이터가 쉽게 변경되어서 test code만큼 test db 관리에 신경서야 된다.
- 복잡한 Assocation
    - FactoryGirl의 [callback](http://robots.thoughtbot.com/post/23039827914/get-your-callbacks-on-with-factory-girl-3-3) method를 이용.

##Controller##

What to test
- 7 CRUD methods
- non CRUD method
- Nested routes
- non-HTML output (i.e. CSV, JSON)

- delete의 invalid
{% highlight ruby %}
sub_menu = double(name: "name")
allow(SubMenu).to receive(:find_by).and_return(sub_menu)
allow(sub_menu).to receive(:destroy).and_return(false)
# allow(sub_menu).to receive(:name).and_return("name")
delete :destroy, id: sub_menu
expect(controller.flash[:danger]).to include("을 삭제할 수 없습니다.")
{% endhighlight %}

sub_menu = double
allow(sub_menu).to receive(:name).and_return("name")
와 
sub_menu = double(name: "name")
이 동일

## Helper ##

예상한 output이 나오는지만 확인
- html을 반환하는 helper method인 경우 output확인에 regex 활용.
    - expect().to match(/regex/)
- session/params에 대한 접근은 `controller.session` 를 이용
- TODO: helper에서 instance variable을 세팅할 경우에 어떻게 값을 확인해야 할까?
    + controller에서처럼 `assigns()`을 사용했지만 안 먹힘.

##Feature(Integration) Test##
**Capybara**
- [visit](https://github.com/jnicklas/capybara#navigating)
- [click](https://github.com/jnicklas/capybara#clicking-links-and-buttons)
- [fill](https://github.com/jnicklas/capybara#interacting-with-forms)
- [css selector](https://github.com/jnicklas/capybara#querying)
- [find](https://github.com/jnicklas/capybara#finding)
- [scope](https://github.com/jnicklas/capybara#scoping)
- [Modal](https://github.com/jnicklas/capybara#modals)

**debug**
Use launcy
- `save_and_open_page` 를 `debugger`처럼 사용하면, firefox에서 method가 불리기 전까지 상태로 보여준다.

##Feature tests with javascript##
capybara에 javascript를 사용할 수 있는 webdriver를 설정 - selenium
selenium은 FireFox를 직접 실행시켜서 진행.
`Gemfile`
{% highlight ruby %}
group :test do
  gem "capybara", "~> 2.8.1"
  gem "selenium-webdriver", "~> 2.53.4"
end
{% endhighlight %}
사용법은 간단. scenario에 `js: true` 옵션을 주면 된다.

- `selenium-webdriver` 버전별로 지원하는 Firefox 버전이 다르다. [selenium changelog](https://github.com/SeleniumHQ/selenium/blob/master/rb/CHANGES)에서 확인 할 수 있다.
- Transactional fixtures do not work with Selenium tests, because Capybara uses a separate server thread, which the transactions would be hidden from. We hence use DatabaseCleaner to truncate our test database.
- [DatabaseCleaner setting](https://github.com/DatabaseCleaner/database_cleaner#rspec-with-capybara-example)
- capybara의 `first` finder를 쓰는 경우에, `find`처럼 javascript가 실행될 수 있게 기다리지 않는다. `Capybara.wait_on_first_by_default = true` 를 `rails_helper.rb`에 추가하면 `find`와 같이 match되는 element가 생길 때 까지 기다린다.
- capybara의 method가 undefined라는 error가 나온다면 `rails_helper.rb`의 `Rspec.config ` 에 `config.include Capybara::DSL`를 추가한다.


- seeds를 loading하지 말고, factorygirl로 만들어서 사용하자.
