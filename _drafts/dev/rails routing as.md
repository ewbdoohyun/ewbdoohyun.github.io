

1.2 Generating Paths and URLs from Code
You can also generate paths and URLs. If the route above is modified to be:

get '/patients/:id', to: 'patients#show', as: 'patient'


as를 이용해서 `patient_path`, `patient_url` method 을 생성할 수 있다.