This folder structure should be suitable for starting a project that uses a database:

* Clone the repo
* `rake generate:migration <Name>` to create a migration
* `rake db:migrate` to run it
* Create models
* ... ?
* Profit

You may need to fiddle around with remotes assuming that you don't want to push to this one (which you probably don't).

## Rundown

```
.
├── Gemfile             # Details which gems are required by the project
├── README.md           # This file
├── Rakefile            # Defines `rake generate:migration` and `db:migrate`
├── config
│   └── database.yml    # Defines the database config (e.g. name of file)
├── console.rb          # `ruby console.rb` starts `pry` with models loaded
├── db
│   ├── dev.sqlite3     # Default location of the database file
│   ├── migrate         # Folder containing generated migrations
│   └── setup.rb        # `require`ing this file sets up the db connection
└── lib                 # Your ruby code (models, etc.) should go here
    └── all.rb          # Require this file to auto-require _all_ `.rb` files in `lib`
```

Datadvanced Homework:


1. How many users are there?

    I used "User.count" to find that there are 49 users (not including the user I created for myself yesterday.
  
2. What are the 5 most expensive items?

    I used "Item.order(price: :desc).limit(5)" to find that the five most expensive items are:
      
        Georgeous Plastic Pants        - 99.44
        Intelligent Cotton Computer  - 90.96
        Small Concrete Pants            - 90.32
        Gorgeous Granite Pants        - 90.24
        Rustic Concrete Pants           - 87.53

3. What is the cheapest electronics item?

    I used "Item.order(price: :asc).where(category: "Electronics").limit(1)" to find that the Rustic Wooden Table is the least expensive Electronics item at 63.3.  I could also use "Item.order(price: :asc).where("category LIKE ?", "%Electronics%").limit(1)" that the cheapest item under Electronics & Movies is Intelligent Wooden Car at 18.48.

4. Who lives at "489 Fritsch Island"? Do they have another address?

    I first used "Address.where(street: "489 Fritsch Island")" to find the User ID associated with that address and then used "User.where(id: 35)" to find that Marty Schmidt lives at 489 Fritsch Island.  I then used "Address.where(user_id: 35)" to find that Marty also has another address on file which is 84642 Bosco Orchard.

5. Correct Tevin Mitchell's New York zip code to 10108.

    I would first use "User.where(first_name: "Tevin")" to find their User ID and then use "Address.where(user_id: 25, state: "New York").limit(1).update_all(zip: 10108);" to change the zip code on Tevin Mitchell's New York address.

6. How much would it cost to buy one of each piece of jewelery?

    I used "Item.where("category LIKE ?", "%Jewelery%").sum(:price)" to find that the cost to buy one of each piece of jewelery is 226.26.

7. How many total items did we sell?

    I used "Order.sum(:quantity)" to find that we sold a total of 817 items.

8. How much was spent on health?

    I used "Item.where(category: "Health")" to determine that Item ID 1 is the only item under the health category and then used "(Item.find(1).price) * (Order.where(item_id: 1).sum(:quantity))" to find that 1796.20 was spent on Health.

9. Simulate buying an item by inserting a User for yourself and an Order for that User (do not include this in the figures above).

    I used "User.create(:id => nil, :first_name => "Patrick", :last_name => "Nielsen", :email => "ptnielsen55@gmail.com")" to insert myself as a user and created an order for myself for 1 Intelligent Steel Chair using "Order.create(:id => nil, :user_id => 52, :item_id => 6, :quantity => 1)".