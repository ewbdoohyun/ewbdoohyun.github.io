
Rails 2.2부터 localization이 되어있다.
local 파일을 받아서 `config/locales` 폴더에 두면 된다.

http://stackoverflow.com/questions/1797357/time-ago-in-words-and-localize


test진행시 `I18n.locale = :ko`를 통해서 locale을 설정할 수 있다.
default_locale을 바꾸면 이전에 영어로 작성해둔 validation message에 대한 test가 fail이 난다.