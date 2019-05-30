regex 사용
- `expect(page.text).to match(/regex/)`

select tag에서 option 선택

http://stackoverflow.com/questions/20134085/how-to-select-option-in-drop-down-using-capybara


# Race problem

- AJAX로 database에 record를 생성하는 경우 하는 경우 race problem이 발생한다.
- ajax가 완료될 때까지 기다리는 helper를 만들어서 사용한다.
{% highlight ruby %}
# spec/support/wait_for_ajax.rb
module WaitForAjax
  def wait_for_ajax
    Timeout.timeout(Capybara.default_max_wait_time) do
      loop until finished_all_ajax_requests?
    end
  end

  def finished_all_ajax_requests?
    page.evaluate_script('jQuery.active').zero?
  end
end

RSpec.configure do |config|
  config.include WaitForAjax, type: :feature
end
{% endhighlight %}
- 사용법
{% highlight ruby %}
expect{
  find('#create_button').click
  wait_for_ajax
}.to change(Item, :count).by(1)
{% endhighlight %}

# Param을 제외한 url만 테스트
`expect(page).to have_current_path(people_path, only_path: true)`

# Fill_in
- HTML
  <input id="name_input">
- spec
  fill_in "name_input"

http://stackoverflow.com/questions/5228371/how-to-get-current-path-with-query-string-using-capybara

[credit : thoughbot](https://robots.thoughtbot.com/automatically-wait-for-ajax-with-capybara)
