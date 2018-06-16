# 암호화 인증서

 - rails ~> '5.2'
 - 'credentials.yml.enc' 파일에만 암호화 보안키가 존재
 - 암호화 인증서는 저장소로 커밋하고 암호화용 키는 절대로 저장소에 커밋해서는 안됨
 - 암호화용 키는 '$ rails new' 명령을 실행할 때 config/master.key 파일에 생성되며 이 파일은 .gitignore 에 추가되어 저장소로 저장되지 않게 됨
 - 암호화 인증서는 config/credentials.yml.enc 파일에 저장됨(이 파일은 직접 변경해서는 안됨)
 - 인증서 추가
~~~
# bash
$ bin/rails credentials:edit
~~~
암호화되지 않은 일반 인증서가 텍스트 에디터 상에 보이게 될 것이다. EDITOR 환경 변수가 지정되어 있지 않다면
~~~
$ EDITOR=vi bin/rails credentials:edit
~~~

## 인증서 해독하기

배포 환경에서의 인증서 사용
~~~
# config/environments/production.rb

config.require_master_key = true
~~~
Rails.application.credentials 값을 호출하면 인증서 내용을 볼 수 있게 된다.
예를 들어 인증서에 다음의 내용이 있다고 가정할 때
~~~
foo: bar
~~~
'$ Rails.application.credentials.foo'는 bar 값을 반환할 것이다.

## 인증서 양식

 - YAML 문법으로 작성한다.
 - 더 이상 development나 production 키를 표시할 필요가 없다.
~~~
# config/environments/production.rb

aws:
  access_key_id: 123
  secret_access_key: 456
~~~
'$ Rails.application.credentials.aws[:access_key_id]'  
'$ Rails.application.credentials.aws[:secret_access_key]'  
  와 같이 호출하여 해당 키 값을 사용할 수 있게 된다.

 - production 키를 사용할 수 없는 것은 아니다.
~~~
production:
  aws:
    access_key_id: 123
    secret_access_key: 456
~~~
'$ Rails.application.credentials.production[:aws][:access_key_id]'  
'$ Rails.application.credentials.production[:aws][:secret_access_key]'  
  와 같이 사용할 수 있게 된다.

## 마스터 키 관리하기

암호화 인증서를 사용하게 되면 암호화되지 않은 보안키를 사용할 때 보다 많은 장점을 제공해 주는데,  
암호화에 사용하는 마스터 키를 적당한 곳에 두어야 한다.  
여기서 언급하는 몇가지 옵션은 rails ~> '5.1' 에서 사용하는 보안키 처리 방식과 비슷하다.  
저장소 소스 코드에 접근할 수 있는 모든 사람이 암호화 인증서를 해독할 수 있기 때문에 마스터 키는 저장소로 커밋해서는 안된다.

 1. master.key 파일을 보안 상태로 업로드하기
    'scp' 또는 'sftp'로 이 파일을 서버로 복사하거나 업로드 할 수 있다. 'Engine Yard' 에서는 한번만 이 과정을 하면 된다.
    이 후 서버 환경을 확장할 경우 새로 추가되는 서버는 자동으로 master.key 파일의 복사본을 가지게 된다.
    마스터 키는 공유 디렉토리로 업로드 해둔다. 여기서의 공유 디렉토리는 공유 파일 시스템이 아니고 여러 차례에 걸쳐 서버로 배포 과정이 진행되더라도 변경되지 않고 그대로 유지되는 디렉토리를 의미한다.
    방법은 배포시 마다 'config.master.key' 파일을 '/path/to/shared/config/masterkey'로 'symlink'를 걸어준다.
    'Engine Yard' 나 'Capistrano'를 사용시 deploy hook에 이러한 작업을 삽입해 둘 수 있다.  
    마스터 키는 파일을 동료 개발자에게 복사해 줘야 할 경우에는 이메일을 암화해서 보내지 않는 한 절대로 이메일로 보내면 안 된다.
    대신에 암호화를 사용하는 비밀번호 관리 툴을 사용할 수 있다.  
  2. 마스터 키 값을 'RAILS_MASTER_KEY' 환경변수로 지정하기
     서버로 파일을 업로드 할 수 없는 상황에서는 이 방법이 유일한 옵션이다. 이 방법은 편리한 대신 환경변수를 사용할 때의 문제점을 확인해 알아둘 필요가 있다.
      이러한 문제점을 해결할 수 있는 여지가 있긴하지만, 'master.key' 파일을 업로드 할 수 있는 상황이 된다면 가능한한 이 옵션을 선택해야 하는 것이 좋다.