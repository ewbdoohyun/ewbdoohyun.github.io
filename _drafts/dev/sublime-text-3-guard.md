

sublime text 2 plugin : 

- Package control을 이용하면, output이 나오지 않음.

수동으로 설치하면 제대로 동작하지만, 이슈가 없진 않다.
- "Start Gaurd" 명령으로 guard를 실행시키지 않으면, output이 보이지 않는 단점.


    cd ~/Library/Application\ Support/Sublime\ Text\ 3/Packages/
    git clone https://github.com/cyphactor/sublime_guard.git Guard


Command 
- Command Pallete(command + shift + p)
    + Start guard
    + End guard
- show guard output : command + shift + c