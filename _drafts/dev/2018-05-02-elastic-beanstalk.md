elastic beanstalk



여러대의 컴퓨터에서

1. .elasticbeanstalk 폴더 셋팅파일 복사
2. eb init
3. eb status 로 확인
4. ssh keypair를 그대로 쓰지 않는다면 eb ssh --setup 으로 새로운 ssh keypair를 생성
	(instance가 다시 시작됨)
