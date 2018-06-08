# Pagination 기능

게시글이 많을 경우 page를 여러 개로 나눌 수 있는 기능

### seed.rb
많은 데이터를 직접 생성하기 보다는 seed파일을 이용해 data를 생성
~~~
# 횟수는 마음대로

100.times do |t|
  uid = [1,2].sample
  Post.create!(title: "#{i+1} 번 째 글입니다.", content: "hi", user_id: uid)
end

User.create!(email: 'first@first.com', password: '123456')
User.create!(email: 'second@second.com', password: '123456')
~~~

### bash
~~~
$ rake db:seed
~~~

### Gemfile
~~~
gem 'will_paginate', '~> 3.1.0'
~~~

### bash
~~~
$ bundle (install)
~~~

### posts_controller.rb
':per_page => N' 은 page 당 몇 N개의 게시물의 존재를 의미한다.
~~~
def index
  ...
  @posts = Post.paginate(:page => params[:page], :per_page => 10)
  ...
end
~~~

### index.html.erb
~~~
<%= will_paginate @posts %>
~~~

### post.rb
~~~
# N장씩 끊어서 볼 수 있는 기능을 지원
self.per_page = N
~~~
