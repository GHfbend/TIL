# DB 보존

### Gemfile
~~~
gem 'paranoia'
~~~

### bash
~~~
$ bundle (install)
~~~
~~~
rails g migration AddDeletedAtToName(# Name = DB 기록을 남길 게시판 컨트롤러 명)
~~~

### /db/migrate/Number_add_deleted_at_to_memos.rb
~~~
def change
  add_column :memos, :deleted_at, :datetime
  add_index :memos, :deleted_at
end
~~~

### memo.rb
~~~
acts_as_paranoid
~~~

