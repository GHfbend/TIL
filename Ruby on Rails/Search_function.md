# 검색기능 구현

- 검색을 데이터베이스에서 진행하게 되어 쿼리문을 작성해서 검색을 해주어야 한다.
- 아래의 순서대로 진행
- 게시글 작성 기능 controller와 model의 name은 Memo

### memos_controller.rb
~~~
def index
  @memos = Memo.all
    if params[:search]
      @memos = Memo.search(params[:search]).order("created_at DESC").page(params[:page]).per(10)
    else
      @memos = Memo.all.order("created_at DESC").page(params[:page]).per(10)
    end
end
~~~

### memos/index
~~~
<%= form_tag(memos_path, :method=> "get", id: "search-form") do %>
  <%= text_field_tag :search.params[:search], placeholder: "Search Posts" %>
  <%= submit_tag "Search" %>
<% end %>
~~~

### memo.rb
~~~
def self.search(search)
  where("content LIKE? OR title LIKE ?", "%#{search}%")
end
~~~