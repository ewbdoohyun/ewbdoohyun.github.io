---
title: 대댓글 기능 gem - act_as_commentable_with_threading
layout: post
comments: true
category: [dev, rails]
--- 


![bebetem.com 대댓글](/public/comment.png)

{: .center}
[[bebetem.com][6] 대댓글]


### 0. Gem 소개

[act_as_commentable_with_threading][1]은 [act_as_commentable][2]에 댓글에 대한 댓글(대댓글) 기능을 추가시킨 gem 입니다.


### 1. 설치법

[Github][1] readme.md 참고하세요.
여기에는 간단하게 코드만 옮겨왔습니다.

Install

    <Gemfile>
        gem 'acts_as_commentable_with_threading'

    <terminal>
    > bundle install
    > rails generate acts_as_commentable_with_threading_migration


설치가 끝나면 `Comment` 모델이 생성되고, `comments` table이 생성됩니다.

schema.rb

    create_table "comments", force: :cascade do |t|
        t.integer  "commentable_id"
        t.string   "commentable_type"
        t.string   "title"
        t.text     "body"
        t.string   "subject"
        t.integer  "user_id",          null: false
        t.integer  "parent_id"
        t.integer  "lft"
        t.integer  "rgt"
        t.datetime "created_at"
        t.datetime "updated_at"
    end

    add_index "comments", ["commentable_id", "commentable_type"], name: "index_comments_on_commentable_id_and_commentable_type", using: :btree

    add_index "comments", ["user_id"], name: "index_comments_on_user_id", using: :btree


`commentable_id`    : 댓글에 대상이 되는 객체의 아이디

`user_id`           : 댓글 소유자 아이디

`parent_id`         : 대댓글의 시작이되는 첫 댓글 아이디 (gem에서는 `root_comment`라 부릅니다.)



### 2. 사용법

댓글에 대상이되는 model에 `acts_ac_commentable`을 추가합니다.

    class Article < ActiveRecord::Base
        acts_as_commentable
    end

댓글을 `build` 할 때는 댓글의 대상이 되는 `@article`과 댓글을 작성하는 유저의 아이디 `@user_who_commented.id` 를 함께 넘겨줍니다.

    @comment = Comment.build_from( @article, @user_who_commented.id, "Hey guys this is my comment!" )

기본적으로 생성되는 `comement.rb`는 다음과 같은 validation이 적용되어 있습니다.

    belongs_to :commentable, :polymorphic => true
    
    belongs_to :user


`:polymorphic => true` 는 commentable이 하나의 model이 아닌 여러 model이 될 수 있게 해줍니다. `schema.rb`에서 `commentable_id`와 `commentable_type` 컬럼이 있었죠. Commentable 이라는 모델은 없지만, polymorphic-association을 통해서 어떤 모델이라도 될 수 있게 됩니다.
Rails guide의 [polymorphic-association][3] 부분을 참고하세요.
이전 버전이긴 하지만 [Ryan Bates의 RailsCast][4]에서도 polymorphic-assocation을 다루고 있습니다.


### 3. Controller

gem에서 controller를 생성해주지는 않습니다. Controller는 직접 구성해야 되는데, 이 gem을 만든 [Dustin Fisher의 블로그][5]에 예제코드가 있습니다.

아래 코드는 [베베템][6] 의 리뷰 시스템을 만들면서 사용한 코드입니다.

    class CommentsController < ApplicationController  
        #TODO: user authentication

        def index
            redirect_to root_path
        end
      
        def new
            @item = Item.find_by(id: params[:item_id])
            @comment = Comment.build_from(@item, current_user.id, "")
        end

        def show
            @comment = Comment.find_by(id: params[:id])
            @item = Item.find_by(id: @comment.commentable_id)
            if current_user
                @new_comment    = Comment.build_from(@item, current_user.id, "")  
            end
        end
        

        def create
            commentable = commentable_type.constantize.find(commentable_id)
            @comment = Comment.build_from(commentable, current_user.id, body)

            respond_to do |format|
              if @comment.save
                make_child_comment
                format.html  {
                  if root_comment_id
                    redirect_to comment_path(root_comment_id)
                  else
                    redirect_to item_path(commentable_id) 
                  end
                }
                format.js
              else
                format.html  { render :action => "new" }
                format.js
              end
            end
        end

        def destroy
            @comment = Comment.find_by(id: params[:id])
            @comment.destroy
            respond_to do |format|
              format.html { redirect_to(:back)}
              format.js
            end
        end

        private

        def comment_params
            params.require(:comment).permit(:body, :commentable_id, :commentable_type, :comment_id, :root_comment_id)
        end

        def root_comment_id
            comment_params[:root_comment_id]
        end

        def commentable_type
            comment_params[:commentable_type]
        end

        def commentable_id
            comment_params[:commentable_id]
        end

        def comment_id
            comment_params[:comment_id]
        end

        def body
            comment_params[:body]
        end

        def make_child_comment
            return "" if comment_id.blank?
            
            parent_comment = Comment.find comment_id
            @comment.move_to_child_of(parent_comment)
        end
    end  



---


[1]: https://github.com/elight/acts_as_commentable_with_threading
[2]: https://github.com/jackdempsey/acts_as_commentable
[3]: http://guides.rubyonrails.org/association_basics.html#polymorphic-associations
[4]: http://railscasts.com/episodes/154-polymorphic-association?view=asciicast
[5]: http://dustinfisher.com/acts-as-commentable-with-threading-gem/
[6]: http://bebetem.com
[7]: https://trello-attachments.s3.amazonaws.com/57de482e882c6cd34e23fceb/5817616271f8bafc57e81547/20b0f1f46d0dbbe2f5a5db1fbb7efeb8/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2016-11-07_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_10.42.02.png
