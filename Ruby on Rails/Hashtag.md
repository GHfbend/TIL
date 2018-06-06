# hashtag 기능 구현

- 아래의 순서대로 진행
- 게시글 작성 기능 controller와 model의 name은 Memo

### bash
- Hashtag 모델 만들기
~~~
$ rails g model hashtag title:string
~~~
- 연결 테이블 만들기
반드시 'Create_join_table_테이블A_테이블B'로 만든다.
~~~
$ rails g migration CreateJoinTableHashtagsMemos Hashtags Memos
~~~

### Memo.rb
~~~
hash_and_belongs_to_many :hashtags
~~~

### Hashtag.rb
~~~
hash_and_belongs
~~~

### bash
~~~
$ rake db:migrate
~~~

### _form.html.erb
- _form에 hashtag 입력창 넣기
- form 안에 field 넣기
- 내용이랑 버튼 사이에 코드 넣기
~~~
<ul>
  <%= f.fields_for :hashtags do |hashtag| %>
    <li>
      <%= hashtag.label :title, 'hashtag' %>
      <%= hashtag.text_field :title %>
    </li>
  <% end %>
</ul>
~~~

### Memo.rb
- hashtag를 nested된 속성으로 받기
~~~
accepts_nested_attributes_for :hashtags
~~~

### Memos_controller.rb
- hashtag 입력 개수 설정
~~~
def new
  ...
  N.title { @memo.hashtags.new }
  ...
end
~~~
~~~
def create
  ...
    N.times do |x|
    tag = hashtag_params[:hashtags_attributes]["{x}"]["title"]
    a = Hashtag.find_or_create_by(title: tag)
    
    @memo.hashtags << a
  ...
end
~~~
~~~
private
...
def hashtag_params
  params.require(:post).permit(hashtags_attributes: [:title])
end
...
~~~

### show.html.erb
- hashtag 보여주기
~~~
<p>
  <strong> hashtag: </strong>
  <% @memo.hashtags.each do |h| %>
   <%= h.title %>
  <% end %>
</p>
~~~