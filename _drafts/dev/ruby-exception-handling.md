
exception도 object
Exception class의 instance

ruby는 exception이 발생하면, 프로그램 종료
exception handler는 except이 발생하면 실행되는 코드 블락
    - `rescue` 로 감싸진 코드 블락

Exception class의 구조
![alt text](http://rubylearning.com/images/exception.jpg)


`raise`를 이용해서, 인위적으로 RuntimeError class의 exception을 만들 수 있다.

Exception handling을 위해서, exception이 발생할 것 같은 코드를 `begin - end` 로 감싸준다.

    # p045handexcp.rb  
    def raise_and_rescue  
      begin  
        puts 'I am before the raise.'  
        raise 'An error has occured.'  
        puts 'I am after the raise.'  
      rescue  
        puts 'I am rescued.' 
      end  
      puts 'I am after the begin block.'  
    end  
    raise_and_rescue  


    >ruby p045handexcp.rb  
    I am before the raise.  
    I am rescued.  
    I am after the begin block.  
    >Exit code: 0  



**rescue** 에 파라메터를 넘겨주지 않으면, StandardError가 기본값이 되고, 파라메터로 특정 exception class를 넘겨주어, exception 마다 handler를 설정할 수 있다.


     begin  
      # -  
    rescue OneTypeOfException  
      # -  
    rescue AnotherTypeOfException  
      # -  
    else  
      # Other exceptions  
    end  


exception을 object에 mapping할 수 있고, message와 backtrace method를 사용할 수 있다.
message : 사람이 읽을 수 있는 에러 메세지 (string)
backtrace : call stack (array of string)



    # p046excpvar.rb  
    begin  
      raise 'A test exception.'  
    rescue Exception => e  
      puts e.message  
      puts e.backtrace.inspect  
    end  


**else**를 이용해 exception이 일어나지 않았을 때만 실행되는 코드를 넣을 수 있고, **ensure**를 이용해서, exception 유무와 관련없이 마지막에 항상 실행되는 코드를 넣을 수 있다.

    begin
      # something which might raise an exception
    rescue SomeExceptionClass => some_variable
      # code that deals with some exception
    rescue SomeOtherException => some_other_variable
      # code that deals with some other exception
    else
      # code that runs only if *no* exception was raised
    ensure
      # ensure that this code always runs, no matter what
    endㅋ


---

http://rubylearning.com/satishtalim/ruby_exceptions.html
http://stackoverflow.com/questions/2191632/begin-rescue-and-ensure-in-ruby