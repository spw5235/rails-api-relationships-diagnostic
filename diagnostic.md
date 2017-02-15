# Rails API Relationships Diagnostic

Place your responses inside the fenced code-blocks where indivated by comments.

1.  Describe a reason why a join tables may be valuable.

  ```md
A joins table is valuable to establish relationships between two tables.  For instance, referencing two table ids in a join table allows further information (regarding either other table) to be ascertained.
  ```

1.  Provide a database table structure and explain the Entity Relationship that
  describes a many-to-many relationship for `Profiles`, `Movies` and `Favorites`
  (Think of Netflix). A `Profile` has a `given_name`, `surname` and `email` and a
  `Movies` have `title`, `release_date`, and `length` and `Favorites` would be the
  join table with references to `Movies` and `Profiles`.

  ```md
    Table 1 - Profile (given_name, surname, email)
    Table 2 - Movies (title, release_date, length)
    Join Table - Favorites (movies, profiles)

    Table 1 to Join Table (One to Many)
    Join Table to Table 2 (Many to many - has_many)
    Table 1 to Table 2 - One to Many through join table
    Join Table belongs_to Table 1
    Join Table belongs_to Table 2
  ```

1.  For the above example, what needs to be added to the Model files?

  ```rb
  class Profile < ActiveRecord::Base
    has_many :movies, through: :favorites
    has_many :favorites

    validates :given_name, presence: true
    validates :surname, presence: true
    validates :email, presence: true
  end
  ```

  ```rb
  class Movie < ActiveRecord::Base
    belongs_to :profiles

    validates :title, presence: true
    validates :release_date, presence: true
    validates :length, presence: true
  end
  ```

  ```rb
  class Favorite < ActiveRecord::Base
    belongs_to :profiles
    belongs_to :movies
  end
  ```

1.  What is the purpose of a serializer? What would our `Profile` serializer look
like to show all movies favorited by a profile on
`http://localhost:3000/profiles/1`

  ```md
    The serializer lists the attributes associated with a table and it defines what information to obtain from a related table
  ```

  ```rb
  class ProfileSerializer < ActiveModel::Serializer

    def profiles
      object.borrowers.pluck(:title)
    end
  end
  ```

1.  What would the command be to _scaffold_ out a **join table** for Favorites from
the above `Movies` and `Profiles`.

  ```sh
    bin/rails generate scaffold favorites reference:movies reference:profiles
  ```

1.  What is `Dependent: Destroy` and where/why would we use it?

  ```md
    Dependent Destroy removes a relationships between tables as stated in a join table.
  ```

1.  Think of **ANY** example where you would have a one-to-many relationship as well
as a many-to-many relationship in an application. You only need to list the
description about the resources and how they relate to one another.

  ```md
    # < Your Response Here >
  ```
