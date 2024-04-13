
Many to Many

Many-to-many associations are a bit more complex and can be handled in two ways, "has and belongs to many" and "has many through" relations.
\
In rails, associations, has_and_belongs_to_many is used to establish a many-to-many relationship between two models. Each instance of one model can be associated with many instances of another model, and vice versa.

A has_and_belongs_to_many association creates a direct many-to-many connection with another model. It is simpler than the other one, as it only requires calling has_and_belongs_to_many from both models.


Example: Let's say, a user can have many different roles and the same role may contain many users, your models would be like this:

class Book < ApplicationRecord       class Title < ApplicationRecord
  has_and_belongs_to_many :titles      has_and_belongs_to_many :books
end                                  end

You will need to create a join table for this association to work. It is a table that connects two different models. The join table is created with rails function create_join_table :book, :title in a separate migration.

class CreateBookTitles < ActiveRecord::Migration
  def change
    create_table :book_titles, id: false do |t|
      t.references :book, index: true, foreign_key: true
      t.references :title, index: true, foreign_key: true
    end
  end
end

This is a very simple approach, but you don't have the direct access to related objects, you can only hold references to two models and nothing else.
------------------------------------------------------------------------------------------------------------
Has Many Through

Another way to define a many-to-many association is to use the has many through association type. Here you should define a separate model, to handle that connection between two different ones.

has_many_through


Using the example of has_and_belongs_to_many association, this time the three models should be written like this:

class User < ApplicationRecord
  has_many :membeships
  has_many :groups, through: :membeships
end

class Membership < ApplicationRecord
  belongs_to :user
  belongs_to :group
end

class Group < ApplicationRecord
  has_many :membeships
  has_many :users, through: :membeships
end

This association will enable you to do things like user.role and to get a list of all connected second model instances. It will also enable you to get access to data specific to the relation between first and second models.