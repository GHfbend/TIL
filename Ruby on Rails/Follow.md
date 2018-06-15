# 팔로우 구현하기

rails 5

### bash
~~~
$ rails g model Follow
~~~

### xxxxxxxxxx_create_follows.rb
~~~
class CreateFollows < ActiveRecord::Migration[5.0]
  def change
    create_table :follows do |t|
    
      # follower: 팔로우를 신청한 사람
      # followed: 팔로우 신청을 받은 사람
      t.integer :follower_id
      t.integer :followed_id
      
      t.timestamps
    end
    
    add_index :follows, :follower_id
    add_index :follows, :followed_id
    add_index :follows, [:follower_id, :followed_id], unique: true
  end
end
~~~

### bash
~~~
$ rake db:migrate
~~~

### Follow.rb
~~~
class Follow < ApplicationRecord
  # 팔로우 신청한 유저
  # 관계 이름 : follower
  # 모델 명 : User
  belongs_to :follower, class_name: "User"
  
  # 팔로우 신청 받은 유저
  # 관계 이름 : followed
  # 모델 명 : User
  belongs_to :followed, class_name: "User"
end
~~~

### User.rb
~~~
class User < ApplicationRecord
  ...
  
  # 관계 이름 : follower_relations(다른 이름으로 변경 가능)
  # 외래키 : followed_id
  # 모델명 : Follow
  has_many :follower_relations, foreign_key: "followed_id", class_name: "Follow"
  
  # 관계 이름 : followers (다른 이름으로 변경 가능)
  # follow_relations를 통해 가져올 값 : follower ( follow.follower )
  has_many :followers, through: :follower_relations, source: :follower
  
  # 팔로잉(위의 팔로우와 비슷하다)
  has_many :follower_relations, foreign_key: "follower_id", class_name "Follow"
  has_many :following, through: :following_relations, source: :followed
end
~~~