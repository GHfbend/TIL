# 관리자 권한

아래의 순서대로 진행

### Gemfile
~~~
gem 'cancancan'
gem 'rolify'
~~~

### bash
~~~
$ bundle (install)
~~~
~~~
$ rails g cancan:ability
$ rails g rolify Role User
~~~
~~~
$ rake db:migrate
~~~

### show.html.erb
~~~
<% if can? :update, @memo %>
  <%= link_to '수정', edit_memo_path %>
<% end %>
<% if can? :destroy, @memo %>
  <%= link_to '삭제', memo_path, method: :delete, data: {confirm: "진짜로 지우시겠습니까?"} %>
<% end %>
~~~

### memos_controller.rb
~~~
# before_action 하단에

load_and_authorize_resource
~~~
~~~
# private 하단에

def is_owner?
  redirect_to root_path unless current_user == @memo.user or :admin
end
~~~

### memo.rb
~~~
resourcify
~~~

### ability.rb
~~~
# 기존의 주석처리된 코드의 주석을 해제하고, 원하는대로 수정한다.

user ||= User.new # guest user (not logged in)
    if user.has_role? :admin
      can :manage, :all #7가지 aciton
    else
      can [:index, :show, :new, :create], Memo
      can [:edit, :update, :destroy], Memo, user_id: user.id
      can [:destroy], Comment, user_id: user.id
    end
~~~

### seed.rb
~~~
# seed에서 권한 미리 설정

User.create!(email: 'admin@admin.com', password: '123456')
User.create!(email: 'second@second.com', password: '123456')
user = User.find(1)
user.add_role :admin
~~~

## console 창에서 권한 설정 test하기

### bash
~~~
$ rails c
$ user = User.find(1)
$ user.add_role :admin
$ user.has_role? :admin
~~~