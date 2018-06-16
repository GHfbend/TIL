# 이미지 파일 업로드

scaffold를 생성한 이후의 환경에서 진행한다.

- Local에 업로드

## 기본
### Gemfile
~~~
gem 'carrierwave'

# 섬네일
gem 'mini_magick'
~~~

### bash
~~~
$ bundle (install)
~~~

### /db/migrate/xxxxxxxxxx_create_posts.rb
~~~
t.string :local_file
t.binary :db_file
t.string :s3_file
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