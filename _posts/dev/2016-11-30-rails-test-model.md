---
title: Rails Test 1 Model
layout: post
comments: true
category: [dev, rails, test]
--- 


Model에서 테스트 해야 할 것.

1. 유효한 attritutes를 넘겼을 때 model의 create method가 유효해야한다.
2. 유효하지 않은 데이터는 유효성 검사를 통과하지 않아야 한다.
3. 클래스 매소드(+scope)와 인스턴스 매소드가 예상한것과 같이 동작한다.

<br>

*item.rb*
    
    class Item < ActiveRecord::Base
        ...
        validate :name, presence: true

<br>

1) 유효한 attritutes를 넘겼을 때 model의 create method가 유효해야한다.

    context 'with valid attritues' do
        it '#create' do

            expect {
                Item.create(name: "A")
            }.to change(Item, :count).by(1)

<br>

2) 유효하지 않은 데이터는 유효성 검사를 통과하지 않아야 한다.

    item = Item.new()
    item.valid?
    expect(item.errors[:name]).to include("can't be blank")

<br>

3) 클래스 매소드, scope, 인스턴스 매소드가 예상한것과 같이 동작한다.

    it '#instance_method' do
    end
    
    it '::class_method' do
    end

<br>


---
[Everyday Rails testing with Rspec][2]

[1]: http://robots.thoughtbot.com/post/23039827914/get-your-callbacks-on-with-factory-girl-3-3
[2]: https://leanpub.com/everydayrailsrspec
