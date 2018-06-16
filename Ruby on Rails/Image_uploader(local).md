# 이미지 파일 업로드

scaffold를 생성한 이후의 환경에서 진행한다.

- Local에 업로드

## scaffold 구성
### bash
~~~
$ rails generate scaffold Post title:string content:text
~~~

### route.rb
~~~
Rails.application.routes.draw do
  root :to => 'post#index'
  resources :posts
end
~~~

### bash
~~~
$ rake db:migrate
~~~

## carrierwave, mini_magick 설치

### Gemfile
~~~
gem 'carrierwave'

# 이미지를 만들고, 수정하고, 지우는 작업들을 도와주는 gem
gem 'mini_magick'
~~~

### bash
~~~
$ bundle (install)
~~~

### /db/migrate/xxxxxxxxxx_create_posts.rb
~~~
t.string :local_file
~~~

### bash
~~~
$ rake db:migrate
~~~

## Local에 업로드
### bash
로컬 업로더 생성
~~~
$ rails g uploader local_file
~~~

### new.html.erb
파일 입력폼
~~~
enctype = "multipart/form-data"
File Upload : <input type="file" name="input_file" accept="image/*">
~~~

### posts_controller.rb
input_file을 모델의 특정 칼럼에 저장
~~~
def create
  ...
  @post.local_file = params[:input_file]
  ...
end
~~~

### index.html.erb
업로드된 파일 출력
~~~
File: <%= image_tag x.local_file %>
~~~
