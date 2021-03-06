{% header %}
  # 2 Creating the First Table in Our Database
{% endheader %}

{% steps %}
  1.  Open Terminal and `cd` to `Projects/bookstore` from your home directory.

      If you have Terminal open from the previous chapter, you should already be
      in `HOME/Projects/bookstore`. Remember how to check what directory you're
      in?

      (`pwd`)

  1.  Run `rails generate model book`.

      This generated a few files, but there are only a couple we're interesed
      in.

  1.  Run `ls -l db/migrate`. You should see a file named something like
      `20161115030350_create_books.rb`.

      20161115030350 is a timestamp generated when the file is created. You're
      file will start with a more recent timestamp...I hope :)

      Let's take a look at this file in your text editor.
{% endsteps %}

{% highlight %}
  › pwd
  /Users/awesomesauce/Projects/bookstore

  › rails generate model book
        invoke  active_record
        create    db/migrate/20161115030350_create_books.rb
        create    app/models/book.rb
        invoke    test_unit
        create      test/models/book_test.rb
        create      test/fixtures/books.yml

  › ls -l db/migrate
  total 8
  -rw-r--r--  1 awesomesauce  staff  131 Nov 14 22:03 20161115030350_create_books.rb
{% endhighlight %}

{% steps %}
  1.  Open the `bookstore` directory in your text editor and open
      `db/migrate/YOUR_TIMESTAMP_create_books.rb`.

  1.  This is a migration file called `CreateBooks`. Migrations are used to
      make changes to the database.

  1.  On line 3 of the `CreateBooks` migration, there's a block called
      `create_table`. Inside this block, you'll make changes to add columns to
      the new `books` table.
{% endsteps %}

{% highlight ruby %}
  class CreateBooks < ActiveRecord::Migration[5.0]
    def change
      create_table :books do |t|

        t.timestamps
      end
    end
  end
{% endhighlight %}

{% steps %}
  1.  The `books` table will need a few columns.

      It will need a string column to store book titles and another string
      column to store book authors.

      The table will also need a column to store book prices. Since keeping
      track of money can be tricky, the column will need to be an integer where
      book prices can be stored in cents. It sounds weird, but you'll have to
      trust us on this one.

  1.  After line 3 of the `CreateBooks` migration, add the following:

      ```
      t.string :title
      t.string :author
      t.integer :price_cents
      ```

  1.  You've added a few things to the `create_table` block, but you might've
      noticed that there was already some code in there:

      ```
      t.timestamps
      ```

      What's `t.timestamps` doing? It's a convinence method added by Rails that
      will add two more columns to the `books` table: `created_at` and
      `updated_at`. They'll be used to store the time when books are created and
      updated.

  1.  Save your changes to the `CreateBooks` migration and go back to Terminal.
{% endsteps %}

{% highlight ruby %}
  class CreateBooks < ActiveRecord::Migration[5.0]
    def change
      create_table :books do |t|
        t.string :title
        t.string :author
        t.integer :price_cents
        t.timestamps
      end
    end
  end
{% endhighlight %}

{% steps %}
  1.  In Terminal, make sure you're in the `bookstore` directory.

  1.  Run `rake db:migrate`.

  1.  Yay! You've just run your first migration!
{% endsteps %}

{% highlight %}
  › pwd
  /Users/awesomesauce/Projects/bookstore

  › rake db:migrate
  == 20161115030350 CreateBooks: migrating ======================================
  -- create_table(:books)
     -> 0.0030s
  == 20161115030350 CreateBooks: migrated (0.0034s) =============================
{% endhighlight %}

{% steps %}
  1.  The migration was just one of the files that was generated by `rails
      generate model book`. It also generated another file:
      `app/models/book.rb`. Let's take a quick look at it.

  1.  Open the `bookstore` directory in your text editor, and open
      `app/models/book.rb`.

  1.  It might not look very exciting, but it's actually pretty powerful. We now
      have a `Book` class. The `Book` class is used to represent individual rows
      in the `books` table.

      Still not impressed? Let's see what you can do with this class.
{% endsteps %}

{% highlight ruby %}
  class Book < ApplicationRecord
  end
{% endhighlight %}

{% steps %}
  1.  Open Terminal and make sure your in the `bookstore` directory.

  1.  Run `rails console`. This will open...the `rails console`.

      The `rails console` is available in all Rails applications. It let's you
      play around with the different things in your applications.

  1.  Now that you're in the `rails console`, let's see what we can do with
      `Book`s.
{% endsteps %}

{% highlight %}
  › pwd
  /Users/awesomesauce/Projects/bookstore

  › rails console
  Loading development environment (Rails 5.0.0.1)
  >>
{% endhighlight %}

{% steps %}
  1.  Let's try creating my favorite book.

  1.  In the `rails console`, run the following code:

      ```
      my_favorite_book = Book.new
      ```

      This will create a new instance of `Book` that we can refer to as
      `my_favorite_book`.

  1.  Now, give `my_favorite_book` a title. Run:

      ```
      my_favorite_book.title = "why's (poignant) Guide to Ruby"
      ```

  1.  `my_favorite_book` now has a title. Don't belive me?! Try running
      `my_favorite_book.title`. It should return "why's (poignant) Guide to
      Ruby".
{% steps %}

{% highlight ruby %}
  >> my_favorite_book = Book.new
  => #<Book id: nil, title: nil, author: nil, price_cents: nil, created_at: nil, updated_at: nil>
  >> my_favorite_book.title = "why's (poignant) Guide to Ruby"
  => "why's (poignant) Guide to Ruby"
  >> my_favorite_book.title
  => "why's (poignant) Guide to Ruby"
(% endhighlight %}

{% steps %}
  1.  Remember those other columns we added to the `books` table? We can set
      those on `my_favorite_book`.

      For example, "why's (poignant) Guide to Ruby" was written by why the lucky
      stiff. Try setting `my_favorite_book`'s author to "why the lucky stiff".

  1.  Although `my_favorite_book` is priceless, you can go ahead and give it a
      price. Remember, we named this column `price_cents`.

  1.  We should probably keep track of the number of copies we have of my
      favorite book. Run the following code to set the quantity:

      ```
      my_favorite_book.quantity = 500
      ```

  1.  Did that work? No?!

      Welcome to your first error!
{% endsteps %}

{% highlight ruby %}
  >> my_favorite_book.author = "why the lucky stiff"
  => "why the lucky stiff"
  >> my_favorite_book.price_cents = 100
  => 100
  >> my_favorite_book.quantity = 500
  NoMethodError: undefined method `quantity=' for #<Book:0x007fdc9453bf60>
      from /Users/awesomesauce/.rvm/gems/ruby-2.3.1/gems/activemodel-5.0.0.1/lib/active_model/attribute_methods.rb:433:in `method_missing'
      from (irb):11
      from /Users/awesomesauce/.rvm/gems/ruby-2.3.1/gems/railties-5.0.0.1/lib/rails/commands/console.rb:65:in `start'
      from /Users/awesomesauce/.rvm/gems/ruby-2.3.1/gems/railties-5.0.0.1/lib/rails/commands/console_helper.rb:9:in `start'
      from /Users/awesomesauce/.rvm/gems/ruby-2.3.1/gems/railties-5.0.0.1/lib/rails/commands/commands_tasks.rb:78:in `console'
      from /Users/awesomesauce/.rvm/gems/ruby-2.3.1/gems/railties-5.0.0.1/lib/rails/commands/commands_tasks.rb:49:in `run_command!'
      from /Users/awesomesauce/.rvm/gems/ruby-2.3.1/gems/railties-5.0.0.1/lib/rails/commands.rb:18:in `<top (required)>'
      from bin/rails:4:in `require'
      from bin/rails:4:in `<main>'
{% endhighlight %}

{% steps %}
  1.  We tried setting quantity on `my_favorite_book`, but we got an error:

      ```
      NoMethodError: undefined method `quantity=' for #<Book:0x007fdc9453bf60>
      ```

  1.  We get a `NoMethodError` for `quantity=` because `quantity` isn't a
      attribute we can set on `Book`.

  1.  If you remember the `CreateBooks` migration, we added a few columns, but
      we never added a column for `quantity`. Let's fix this!
{% endsteps %}

{% highlight ruby %}
  class CreateBooks < ActiveRecord::Migration[5.0]
    def change
      create_table :books do |t|
        t.string :title
        t.string :author
        t.integer :price_cents
        t.timestamps
      end
    end
  end
{% endhighlight %}
