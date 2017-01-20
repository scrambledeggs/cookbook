# Ruby

- generally follow community style guide: [bbatsov/ruby-style-guide](https://github.com/bbatsov/ruby-style-guide)

# Rails
- Grouping and order on top of rails models
```
class ModelName < ApplicationRecord
  # enums
  enum type: [:a, :b, :c, :d]
  
  # relationships
  belongs_to :parent
  has_many :children
  
  # validations
  validates :name, presence: true
  
  # scopes
  scope :deleted, -> { where(deleted: true) }
end
```
