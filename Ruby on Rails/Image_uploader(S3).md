# 이미지 파일 업로드

- AWS S3에 업로드

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

## carrierwave, mini_magick, fog 설치
### Gemfile
~~~
gem 'carrierwave'

# aws 사용을 위해 필요(클라우드에 파일의 저장을 쉽게할 수 있도록 도와주는 gem)
gem 'fog-aws'

# 이미지를 만들고, 수정하고, 지우는 작업들을 도와주는 gem
gem 'mini_magick'
~~~

### bash
~~~
$ bundle (install)

# minimagick을 사용하기 위해서 image_magick을 설치해준다
$ sudo apt-apt-get -y install imagemagick

$ rake db:migrate
~~~

## Uploader 셋팅
### bash
~~~
$ rails generate uploader S3
~~~

### s3_uploader.rb

 - mini_magick 설정
 - storage를 fog로 변경

~~~
class S3Uploader < CarrierWave::Uploader::Base

  # 이미지를 조정할 수 있는 툴 설정
  # Include RMagick or MiniMagick support:
  # include CarrierWave::RMagick
  include CarrierWave::MiniMagick

  # 이미지를 저장할 장소의 종류를 설정
  # Choose what kind of storage to use for this uploader:
  # storage :file
  storage :fog

  #이미지가 저장되는 위치
  # Override the directory where uploaded files will be stored.
  # This is a sensible default for uploaders that are meant to be mounted:
  def store_dir
    "uploads/#{model.class.to_s.underscore}/#{mounted_as}/#{model.id}"
  end

  # 요청한 이미지가 없을 때 대체해서 사용하는 default 이미지 설정
  # Provide a default URL as a default if there hasn't been a file uploaded:
  # def default_url
  #   # For Rails 3.1+ asset pipeline compatibility:
  #   # ActionController::Base.helpers.asset_path("fallback/" + [version_name, "default.png"].compact.join('_'))
  #
  #   "/images/fallback/" + [version_name, "default.png"].compact.join('_')
  # end

  # 이미지를 저장할 사이즈 조정
  # Process files as they are uploaded:
  # process scale: [200, 300]
  #
  # def scale(width, height)
  #   # do something
  # end

  # 여러가지 이미지의 버전 설정
  # Create different versions of your uploaded files:
  # version :thumb do
  #   process resize_to_fit: [50, 50]
  # end

  # 저장될 파일들의 확장자 설정
  # Add a white list of extensions which are allowed to be uploaded.
  # For images you might use something like this:
  def extension_whitelist
    %w(jpg jpeg gif png)
  end

  # 저장되는 파일의 이름 설정
  # Override the filename of the uploaded files:
  # Avoid using model.id or version_name here, see uploader/store.rb for details.
  # def filename
  #   "something.jpg" if original_filename
  # end
end
~~~

## Model 셋팅

 - Post model에 각각의 post가 이미지를 가져야 하므로 이미지 링크가 저장될 공간을 확보해야 함
 - 기존 model에 새로운 문자열 저장 공간(column)을 만들어야 함
 - Post model이 uploader를 사용할 수 있도록 mount 명령어를 추가해야 함

### bash
~~~
$ rails g migration add_s3_to_posts image:string
$ rake db:migrate
~~~

### post.rb
~~~
class Post < ActiveRecord::Base
  mount_uploader :image, S3Uploader
end
~~~

## config 설정

fog-aws를 사용하기 위해서는 'config/initializers'에 fog.rb 파일을 직접 생성해야 함

### config/initializers/fog.rb
~~~
CarrierWave.configure do |config|
  config.fog_provider = 'fog/aws'                        # required
  config.fog_credentials = {
    provider:              'AWS',                        # required
    aws_access_key_id:     'xxx',                        # required
    aws_secret_access_key: 'yyy',                        # required
    region:                'ap-northeast-2',             # optional, defaults to 'us-east-1'
  }
  config.fog_directory  = 'name_of_directory'            # required
end
~~~
 - 위 코드에서 id, key, region, name_of_directory 부분을 사용자의 설정에 맞게 변경
 - 설정 변경에 관해서는 아래의 링크 참고(추후 추가 예정)

## View에서 이미지 파일을 올리고 볼 수 있게 하는 parameter 셋팅
### _form.html.erb
~~~
<%= form_for(@post) do |f| %>
  <% if @post.errors.any? %>
    <div id="error_explanation">
      <h2><%= pluralize(@post.errors.count, "error") %> prohibited this post from being saved:</h2>

      <ul>
      <% @post.errors.full_messages.each do |message| %>
        <li><%= message %></li>
      <% end %>
      </ul>
    </div>
  <% end %>

  <div class="field">
    <%= f.label :title %><br>
    <%= f.text_field :title %>
  </div>
  <div class="field">
    <%= f.label :content %><br>
    <%= f.text_area :content %>
  </div>
  <div class="field">
    <%= f.label :image %><br>
    <%= f.file_field :image, accept: 'image/png,image/gif,image/jpeg' %>
  </div>
  <div class="actions">
    <%= f.submit %>
  </div>
<% end %>
~~~

### posts_controller.rb
~~~
class PostsController < ApplicationController
  private
    # Never trust parameters from the scary internet, only allow the white list through.
    def post_params
      params.require(:post).permit(:title, :content, :image)
    end
end
~~~

### index.html.erb
~~~
<p id="notice"><%= notice %></p>

<h1>Listing Posts</h1>

<table>
  <thead>
    <tr>
      <th>Title</th>
      <th>Content</th>
      <th colspan="3"></th>
    </tr>
  </thead>

  <tbody>
    <% @posts.each do |post| %>
      <tr>
        <td><%= post.title %></td>
        <td><%= post.content %></td>
        <td><%= image_tag("#{post.image}") %></td>
        <td><%= link_to 'Show', post %></td>
        <td><%= link_to 'Edit', edit_post_path(post) %></td>
        <td><%= link_to 'Destroy', post, method: :delete, data: { confirm: 'Are you sure?' } %></td>
      </tr>
    <% end %>
  </tbody>
</table>

<br>
<%= link_to 'New Post', new_post_path %>
~~~

### show.html.erb
~~~
<p id="notice"><%= notice %></p>

<p>
  <strong>Title:</strong>
  <%= @post.title %>
</p>

<p>
  <strong>Content:</strong>
  <%= @post.content %>
</p>
<p>
  <strong>사진:</strong>
  <%= image_tag("#{@post.image}") %>
</p>

<%= link_to 'Edit', edit_post_path(@post) %> |
<%= link_to 'Back', posts_path %>
~~~

## 효율적인 이미지 사용
### app/uploaders/s3_uploader.rb
~~~
class S3Uploader < CarrierWave::Uploader::Base
  include CarrierWave::MiniMagick
  
  version :detail do
    process :resize_to_fit => [#, #]
  end
  version :main do
      process :resize_to_fill => [#, #]
  end
end
~~~
 - CarrierWave는 detail버전의 이미지와 main이라는 본래의 이미지를 아래의 조건에 따라 S3 안에 저장한다
 - 버전별 사용 방법: 기존 이미지 경로에 ''.detail', '.main' 추가
 - 'resize_to_fill': 입력 값 비율에 맞게 사진을 맞춤
 - 'resize_to_fit': 입력 값 중 짧은 길이에 맞춰서 나머지 부분을 조절