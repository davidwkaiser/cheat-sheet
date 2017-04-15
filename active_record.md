class Order < ActiveRecord::Base  
  belongs_to :customer, :class_name => "Patron",  
    :foreign_key => “patron_id”  
end  

:source is used (optionally) to define the associated model name when you're using has_many  through; :class_name is used (optionally) in a simple has many or belongs to relationship.  

# app/models/post.rb  
class Post < ActiveRecord::Base  
  has_many :post_authorings, :foreign_key => :authored_post_id  
  has_many :authors, :through => :post_authorings, :source => :post_author  
  belongs_to :editor, :class_name => "User"  
end  

# app/models/user.rb  
class User < ActiveRecord::Base  
  has_many :post_authorings, :foreign_key => :post_author_id  
  has_many :authored_posts, :through => :post_authorings  
  has_many :edited_posts, :foreign_key => :editor_id, :class_name => "Post"  
end  

# app/models/post_authoring.rb  
class PostAuthoring < ActiveRecord::Base  
  belongs_to :post_author, :class_name => "User"  
  belongs_to :authored_post, :class_name => "Post"  
end  

#Polymorphic  
# app/models/comments.rb  
class Comment < ActiveRecord::Base  
  belongs_to :commentable, :polymorphic => true  
end  

# app/models/post.rb  
class Post < ActiveRecord::Base  
  has_many :comments, :as => :commentable  
end  

# app/models/picture.rb  
class Picture < ActiveRecord::Base  
  has_many :comments, :as => :commentable  
end  

class CreateComments < ActiveRecord::Migration  
  def change  
    create_table :comments do |t|  
      t.string :title  
      t.text :content  
      t.integer :commentable_id  
      t.string :commentable_type  
      t.timestamps  
    end  
  end  
end  

#self-joins  
class Employee < ActiveRecord::Base  
  has_many :subordinates, class_name: "Employee",  
                          foreign_key: "manager_id"  

  belongs_to :manager, class_name: "Employee"  
end  

