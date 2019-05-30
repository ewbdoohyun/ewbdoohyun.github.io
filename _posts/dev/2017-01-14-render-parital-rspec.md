---
title: Rspec - partial rendering 테스트 하기
layout: post
comments: true
category: [dev, rspec]
--- 


view에서 유저일 때는 광고를 보여주지 않는 부분을 테스트.

    <%= render partial: "layouts/gdn_main_top" if not logged_in? %>


controller.rspec

    describe 'Visitor access' do
        describe '#show' do
          render_views
          it 'shows gdn ads' do
            get :show, id: @category
            expect(response).to render_template(partial: "layouts/_gdn_main_top")
          end
        end
      end
    
      describe 'Use access' do
        before(:each) do
          log_in_as(create(:user))
        end
        describe '#show' do
          render_views
          it 'shows gdn ads' do
            get :show, id: @category
            expect(response).not_to render_template(partial: "layouts/_gdn_main_top")
          end
        end
      end

controller spec에서는 view를 rendering하지 않는다.
`render_views` 를 통해 특정 테스트에서 view를 rendering할 수 있다.

partial이 rendering되었는지는 `render_template` 에 partial 옵션을 줘서 확인한다.


---

[rspec doc - render views][1]

[1]: https://www.relishapp.com/rspec/rspec-rails/v/3-0/docs/controller-specs/render-views
