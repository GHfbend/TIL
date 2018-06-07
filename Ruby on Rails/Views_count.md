# 조회수 기능 구현

## 조회수

### Gemfile
~~~
gem 'impressionist'
~~~

### bash
~~~
$ bundle (install)
~~~
~~~
$ rails g impressionist
~~~
~~~
$ rake db:migrate
~~~

### memo.rb
~~~
has_many :impressions, :as=>:impressionable
~~~
~~~
def impression_count
  impressions.size
end
~~~
~~~
def unique_impression_count
  impressions.group(:ip_address).size.keys.length
end
~~~

### memos_controller.rb
~~~
before_action :log_impression, only: [:show]
~~~
~~~
def log_impression
  @hit_memo = Memo.find(params[:id])
  @hit_memo.impressions.create(ip_address: request.remote_ip, user_id:current_user.id)
end
~~~

### index.html.erb
~~~
<%= "#{x.unique_impression_count}"%>
~~~

## 조회수 높은 순으로 정렬하기

### bash
~~~
$ rails g migrate AddimpressionsCountToMemo impressions_count:int
~~~
~~~
$ rake db:migrate
~~~

### memo.rb
~~~
is_impressionable :counter_cache => true, :unique => true
~~~

### memos_controller.rb
~~~
def index
  ...
  @memo = Memo.order("impressions_count DESC")
  ...
end
~~~