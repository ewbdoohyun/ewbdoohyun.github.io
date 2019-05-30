---
title: 좋아요/싫어요 투표 기능 gem - acts_as_votable
layout: post
comments: true
category: [dev, rails]
--- 


![bebetem.com 리뷰 투표](/public/voting.png)

{: .center}
[[bebetem.com][6] 리뷰 투표]


### 0. Gem 소개

[acts_as_votble][1]은 손쉽게 좋아요/싫어요 등의 투표 기능을 구현할 수 있게 해주는 Gem입니다.



### 1. 설치법

[Github][1] readme.md 참고하세요.
여기에는 간단하게 코드만 옮겨왔습니다.



    //Gemfile
        ...
        gem 'acts_as_votable'
        ...

    //terminal

        bundle install
        rails generate acts_as_votable:migration
        rake db:migrate


마이그레이션이 끝나면 *votes* table이 생성됩니다.


    //schema.rb

        create_table "votes", force: :cascade do |t|
            t.integer  "votable_id"
            t.string   "votable_type"
            t.integer  "voter_id"
            t.string   "voter_type"
            t.boolean  "vote_flag"
            t.string   "vote_scope"
            t.integer  "vote_weight"
            t.datetime "created_at"
            t.datetime "updated_at"
        end
    
        add_index "votes", ["votable_id", "votable_type", "vote_scope"], name: "index_votes_on_votable_id_and_votable_type_and_vote_scope", using: :btree

        add_index "votes", ["voter_id", "voter_type", "vote_scope"], name: "index_votes_on_voter_id_and_voter_type_and_vote_scope", using: :btree

**votable_id**    : 투표 대상이 되는 객체의 아이디 <br>
**votable_type**  : polymorphic association에 사용. 투표 대상이 되는 객체의 클래스명 <br>
**voter_id**      : 투표하는 주체가 되는 객체의 아이디 <br>
**voter_type**    : polymorphic association에 사용. 투표 주체가 되는 객체의 클래스명 <br>
**vote_flag**     : true이면 좋아요. false이면 싫어요입니다. <br>

[act_as_votable 소스코드][3]를 살펴보면 아래처럼 *votable*과 *voter*에 대해서 polymorphic association으로 구현되어 있습니다. 어떤 클래스든 votable과 voter가 될 수 있는거죠.
    
    //lib/acts_as_votable/vote.rb
        belongs_to :votable, :polymorphic => true
        belongs_to :voter, :polymorphic => true



### 2. 사용법
  

투표의 대상이 되는 model에 *acts_as_votable*을 추가합니다.

    class Post < ActiveRecord::Base
        acts_as_votable
    end

투표 주체가 되는 model에 *acts_as_voter*을 추가합니다.

    class User < ActiveRecord::Base
      acts_as_voter
    end



*acts_as_votable* 로 추가되는 method

투표기능

    //좋아요
    @post.liked_by @user1
    @user1.likes @post

    //좋아요 취소
    @post.unliked_by @user1

    //싫어요
    @post.disliked_by @user1

    //싫어요 취소
    @post.undisliked_by @user1    

투표결과

    // 총 투표 수
    @post.votes_for.size

    // 좋아요 수
    @post.get_likes.size

    // 싫어요 수
    @post.get_dislikes.size

투표결과리스트

    // 유저가 좋아요한 Post 리스트
    @user.get_up_voted Post

    // 유저가 싫어요한 Post 리스트
    @user.get_down_voted Post


추가 기능들 <br>
- [투표에 가중치 설정][2]


### 3. Controller

    //comments_controller.rb
    
    class CommentController < ApplicationController  
        ...
        def like
            @comment = Comment.find_by(id: params[:comment_id])
            if current_user
              @comment.liked_by(current_user)
            end
        
            respond_to do |format|
              format.js {
                render "comments/votes"
              }
            end
          end
        
          def dislike
            @comment = Comment.find_by(id: params[:comment_id])
            if current_user
              @comment.disliked_by(current_user)
            end
        
            respond_to do |format|
              format.js {
                render "comments/votes"
              }
            end
          end
        ...
    end


    //routes.rb
    
    resources :comments do
        get 'like' => "comments#like"
        get 'dislike' => "comments#dislike"
      end


## 4. Cached-attributes

    class AddCachedVotesToPosts < ActiveRecord::Migration
      def self.up
        add_column :posts, :cached_votes_total, :integer, :default => 0
        add_column :posts, :cached_votes_score, :integer, :default => 0
        add_column :posts, :cached_votes_up, :integer, :default => 0
        add_column :posts, :cached_votes_down, :integer, :default => 0
        add_column :posts, :cached_weighted_score, :integer, :default => 0
        add_column :posts, :cached_weighted_total, :integer, :default => 0
        add_column :posts, :cached_weighted_average, :float, :default => 0.0
        add_index  :posts, :cached_votes_total
        add_index  :posts, :cached_votes_score
        add_index  :posts, :cached_votes_up
        add_index  :posts, :cached_votes_down
        add_index  :posts, :cached_weighted_score
        add_index  :posts, :cached_weighted_total
        add_index  :posts, :cached_weighted_average
    
        # Uncomment this line to force caching of existing votes
        # Post.find_each(&:update_cached_votes)
      end
    
      def self.down
        remove_column :posts, :cached_votes_total
        remove_column :posts, :cached_votes_score
        remove_column :posts, :cached_votes_up
        remove_column :posts, :cached_votes_down
        remove_column :posts, :cached_weighted_score
        remove_column :posts, :cached_weighted_total
        remove_column :posts, :cached_weighted_average
      end
    end


## 5. sorting by voting score

    order(cached_votes_score: :desc)      // score: like(+1) 와 dislike(-1)를 합한 값


---

[act_as_votable github][1]

[1]: https://github.com/ryanto/acts_as_votable
[2]: https://github.com/ryanto/acts_as_votable#adding-weights-to-your-votes
[3]: https://github.com/ryanto/acts_as_votable/blob/master/lib/acts_as_votable/vote.rb
[6]: http://bebetem.com
