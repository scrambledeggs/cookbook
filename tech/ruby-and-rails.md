# Ruby

- generally follow community style guide: [bbatsov/ruby-style-guide](https://github.com/bbatsov/ruby-style-guide)
- [rubocop.yml template](rubocop.yml)
- prefer rspec over test-unit

# Rails
- Keep `README.md` always updated
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
- Custom code or PORO (validators, services, etc)
  - Add application specific code under `app/`
  - Add code that can be extracted for use in other projects to `lib/`
    - Ask the question: Can I use this code in my Tinder clone? If yes, it goes in `lib`

- Database
  - Ensure db:migrate runs properly
  - Ensure db:rollback runs properly
  - Ensure db:seed runs properly (They get outdated fast!). Alternatively, provide a way to seed a newcomer's project.
  - Ensure painless turnovers
