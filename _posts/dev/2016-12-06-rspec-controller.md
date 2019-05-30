---
title: Rails Controller Test with Rspec
layout: post
comments: true
category: [dev, rails, test, rspec]
--- 

### Controller 테스트 범위

1. Controller의 CRUD 관련 매소드가 예상한것과 같이 동작한다.
2. CRUD외 다른 메소드가 예상한 것과 같이 동작한다.
3. nested attribute
4. nested routing

<br>  

### Controller method를 호출 하는 방법

    <HTTP verb> <controller method name> <params>

    get :index

    get :edit, id: 1

    get :show, id: 1

    post :create, name: 'a', brand: 'b'

    delete :destroy, id: 1

<br>

### Controller method에서 선언된 변수에 접근하는 방법

`assign(:variable)`

    expect(assign(:items)).to be [item1, item2]

<br>

### Controller method로 부터 반환되는 값

`response`

    expect(response).to redirect_to user_path
    expect(response).to render_template :index

<br>
<br>

### 1. CRUD method test

#### a. GET Request

1. 원하는 데이터가 instance variable에 제대로 할당되어야 한다.
2. 액션에 맞는 템플릿을 렌더링하여야 한다.

Method : #index, #new, #edit, #show


    describe'#show'do
        it "assigns the requested contact to @contact" do
            contact = create(:contact)
            get :show, id: contact
            expect(assigns(:contact)).to eq contact
        end
        it "renders the :show template" do
            contact = create(:contact)
            get :show, id: contact 
            expect(response).to render_template :show
        end 
    end

<br>

#### b. POST request

1. valid parameter가 주어졌을 때
    - Database에 새로운 record 생성
    - 원하는 path로 redirect
    - (옵션) Flash message 제대로 설정
2. invalid parameter가 주어졌을 때
    - Database에 새로운 record를 생성하지 않음
    - 원하는 contents rendering
    - (옵션) Flash message 제대로 설정

_

    describe '#create.html' do
      context "with valid attributes" do
        it "saves the new item into the database" do
          expect {
            post :create, item: attributes_for(:item)
            }.to change(Item, :count).by(1)
        end
        it "redirects to items#index" do
          post :create, item: attributes_for(:item)
          expect(response).to redirect_to items_path
        end
        it "sets flash message" do
          post :create, item: attributes_for(:item)
          expect(flash[:success]).to match("create")
        end
      end
      context "with invalid attributes" do
        it "does not save the new item into the database" do
          expect {
            post :create, item: attributes_for(:invalid_item)
          }.not_to change(Item, :count)
        end
        it "re-renders the :new template" do
          sub_menu = create(:sub_menu)
          post :create, item: attributes_for(:invalid_item)
          
          expect(response).to render_template :new
        end
      end


Ajax request

    xhr :post, :create, item: attributes_for(:item)

status check

    expect(response.status).to be(404)

<br>

#### c. PATCH request

1) valid attributes
  - requested object
  - DB update
  - redirect
  - Flash message (success)

2) invalid attributes
  - DB update (not changed)
  - render
  - Flash message (error)

업데이트된 record값을 다시 가져오기 : `reload`

    describe 'update' do
      context "with valid attributes" do
        it "locates the requested @item" do
          patch :update, id: @item, item: attributes_for(:item)
          expect(assigns(:item)).to eq(@item)
        end
        it "changes @item's attributes" do
          patch :update, id: @item, item: attributes_for(:item, name: "!")
              
          @item.reload
    
          expect(@item.name).to eq "!"
        end
        it "redirects to item#index" do
          patch :update, id: @item, item: attributes_for(:item)
          expect(response).to redirect_to items_path
        end
      end
      context "with invalid attributes" do
        it "does not change the @item's attributes" do
          name = @item.name
          patch :update, id: @item, item: attributes_for(:invalid_item)
          expect(@item.reload.name).to eq(name)
        end
        it "re-renders the :edit template" do
          patch :update, id: @item, item: attributes_for(:invalid_item)
          expect(response).to render_template :edit
        end
        it 'sets flash message' do
          patch :update, id: @item, item: attributes_for(:invalid_item)
          expect(flash[:danger][:name]).not_to be_nil
        end
      end
    end


<br>

#### d. DELETE request

1. 잘 지워졌는
2. 지우고나서 redirect

_

    describe '#destroy' do
      it "deletes the item" do
        expect {
          delete :destroy, id: @item
        }.to change(Item, :count).by(-1)
      end
      it "redirects to item#index" do
        delete :destroy, id: @item
        expect(response).to redirect_to items_path
      end
    end


###2. Non-CRUD method test
###3. Nested Attributes
###4. Nested Routes


---
