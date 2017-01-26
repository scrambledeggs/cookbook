# Rails
- Keep `README.md` always updated
- Grouping and order on top of `ActiveRecord` models:
```
class ModelName < ApplicationRecord
  # includes
  include ModuleName

  # relationships
  belongs_to :parent
  has_many :children

  # attributes
  attr_accessor :fieldname

  # validations
  validates :name, presence: true

  # enums
  enum type: [:a, :b, :c, :d]

  # scopes
  scope :deleted, -> { where(deleted: true) }
end
```
- Custom code or PORO<sup>1</sup> (validators, services, etc):
  - Add application specific code under `app/`
  - Add code that can be extracted for use in other projects to `lib/`
    - Ask the question: Can I use this code in my Tinder clone? If yes, it goes in `lib`

## Database
  - Ensure db:migrate runs properly
  - Ensure db:rollback runs properly
  - Ensure db:seed runs properly (They get outdated fast!). Alternatively, provide a way to seed a newcomer's project.
  - Ensure painless turnovers

## "Service" Objects
  - A _service object_ is a PORO that models business logic so that it can be reused across multiple callers (for example from controllers, rake tasks, background jobs and even other services!).
  - Name these classes as _`NounVerber`_
    - `CreditCardCharger`
  - Expose a constructor that sets up initialization configuration
    - `CreditCardCharger.new(processor: :braintree)`
  - Expose an instance method named _`call`_ and send data parameters to this
    - `CreditCardCharger.new(processor: braintree).call('4111111111111111','123','11/28')`
    - Call returns a `Result` object which can encapsulate the success, errors and instance
    ```
      result = CreditCardCharger.new(processor: :Braintree).call(card_details)
      if result.success?
        Rails.log("Successful trans: #{result.transaction}")
      else
        Rails.error("Error in transaction: #{result.errors}")
      end
    ```
      - Prefer to use [immutable-struct](https://github.com/stitchfix/immutable-struct) for this.
  - Keep in `app/services` folder (see _custom code_ above)

---
<sup>1</sup> PORO = **P**lain **O**ld **R**uby **O**bject.
  > The distinction between a PORO (which stands for Plain Old Ruby Object) and a not-PORO is not a technical one. All objects are technically Ruby object. Instead, the term PORO is sometimes used when talking about large frameworks like Rails which tend to use "big", "complex" objects like ActiveRecord models for much of its logic. These large objects tend to contain a large amount of logic and defined behavior.

  > -via [Holger Just/Stack Overflow](http://stackoverflow.com/a/39129461)
