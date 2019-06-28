## Rails migrations là gì, tại sao lại có nó và dùng nó như thế nào?

Theo như trang hướng dẫn chính thức có giới thiệu thì migration là một cách tiện lời để thay đổi database schema dễ dàng và thống nhất theo thời gian. Với mỗi sự thay đổi trong cấu trúc db từ đầu đến cuối đều được phản ánh hết trong migration. Từng file là từng phiên bản, từng lát cắt thời gian của db. Vì migration dùng ruby DSL nên không cần thiết phải tự viết SQL, giúp cho các thay đổi cũng như schema độc lập với database.
 
Với những định nghĩa như trên thì migration giúp ích rất nhiều cho việc quản lý cấu trúc db, hợp tác làm việc cho nhiều lập trình viên trong cùng một nhóm, không phải mó nhiều tới SQL hay database, trong những dự án mình làm thì việc điều chỉnh cấu trúc database hiếm khi phải tự viết tay. Thêm một điểm đó là ruby khá thân thiện sẽ nên dễ đọc dễ hiểu và dễ chia sẻ với nhóm.

Cứ với mỗi phiên bản migration là đang phản ánh tình trạng hiện tại của schema từ version đó trở về trước, mỗi version được phản ánh qua nhiều chức năng như tạo bảng, xóa bảng, thêm cột, xóa cột, thêm _index_, _constraint_,... Cùng với việc chạy migration, ActiveRecord cũng đồng thời cập nhật file `db/schema.rb` phản ánh cấu trúc của database.

Đó là vài điểm theo kinh nghiệm mình thấy hay và mình nghĩ để thể hiện điều đó rõ nét nhất là thông qua ví dụ, thực hành.
 
## Trong thực tiễn
### Tạo migration

Tên file migration tuân thủ theo qui tắc *số_cộng_tên_migration.rb*. Các file migration được sắp xếp theo tên file theo thứ tự chữ cái. Lệnh `rails db:migrate` để migrate database. Khi migrate, rails sẽ dựa vào thứ tự các file để thực hiện migration, nếu sai thứ tự sẽ xảy ra nhiều đụng chạm nghiêm trọng phát sinh lỗi vì thế rails có hỗ trợ cho ta vài lệnh tạo migration kèm theo con số ở đầu file được sinh ra từ thời gian theo UTC, ví dụ *20190511031142_create_posts.rb* được tạo bằng lệnh
```
rails g migration CreatePosts
```
Trong file được tạo ra sẽ chứa đoạn code như này (đoạn comment là tự viết)
```ruby
class CreatePosts < ActiveRecord::Migration[6.0]
 def change
   create_table :posts do |t|
     # t.references :user, null: false, foreign_key: true
     # t.string :title
     # t.text :body

     # t.timestamps
   end
 end
end
```
Lệnh tạo migration còn hỗ trợ định nghĩa trước các cột, ví dụ như
```
rails g AddCategoryToPosts category:references views:integer
```
Một lệnh khác khá hữu ích đó là lệnh tạo model
```
rails generate model Product name:string description:text
```
cho ta file migration như này
```ruby
class CreateProducts < ActiveRecord::Migration[5.0]
  def change
    create_table :products do |t|
      t.string :name
      t.text :description
 
      t.timestamps
    end
  end
end
```

### Viết và chạy migration

Để hỗ trợ viết migration, rails cung cấp rất nhiều hàm ví dụ như `create_table`, `add_column`, `remove_column`, ... các bạn có thể tham khảo ở những link sau: [ActiveRecord::ConnectionAdapters::SchemaStatements](https://edgeapi.rubyonrails.org/classes/ActiveRecord/ConnectionAdapters/SchemaStatements.html), [ActiveRecord::ConnectionAdapters::TableDefinition](https://edgeapi.rubyonrails.org/classes/ActiveRecord/ConnectionAdapters/TableDefinition.html), [ActiveRecord::ConnectionAdapters::Table](https://edgeapi.rubyonrails.org/classes/ActiveRecord/ConnectionAdapters/Table.html)

Điểm đặc biệt quan trọng ở đây đó là migration cho phép *quay ngược về quá khứ* có nghĩa là quay trở lại phiên bản trước đó của database, lệnh để thực thi là `rails db:rollback`, hỗ trợ thêm biến `STEP=số` để quay ngược lại *số* verion của migration. Lệnh này là một cách viết khác cho lệnh `rails db:migration VERSION=số_đầu_file`, cùng với đó là lệnh `rails db:migrate:redo STEP=số` để rollback và migrate lại.
 
Để có thể rollback thì rails phải biết rollback như thế nào. Đối với hàm `change` trong migration, khi gọi hàm trong hàm này thì rails sẽ tự động rollback nếu bản thân hàm được gọi đó có thể rollback được, nếu không sẽ raise lỗi `ActiveRecord::IrreversibleMigration`. Khi đó ta phải cung cấp rails thông tin để rollback lại bằng hàm `reversible`, ví dụ
```ruby
class ExampleMigration < ActiveRecord::Migration[5.0]
  def change
    create_table :distributors do |t|
      t.string :zipcode
    end
 
    reversible do |dir|
      dir.up do
        # add a CHECK constraint
        execute <<-SQL
          ALTER TABLE distributors
            ADD CONSTRAINT zipchk
              CHECK (char_length(zipcode) = 5) NO INHERIT;
        SQL
      end
      dir.down do
        execute <<-SQL
          ALTER TABLE distributors
            DROP CONSTRAINT zipchk
        SQL
      end
    end
 
    add_column :users, :home_page_url, :string
    rename_column :users, :email, :email_address
  end
end
```
Hoặc không ta có thể dùng cách cũ của rails là định nghĩa hai hàm `up`, `down` để nói rõ cho rails biết migrate làm gì và rollback làm gì.
 ```ruby
 class ExampleMigration < ActiveRecord::Migration[5.0]
  def up
    create_table :distributors do |t|
      t.string :zipcode
    end
 
    # add a CHECK constraint
    execute <<-SQL
      ALTER TABLE distributors
        ADD CONSTRAINT zipchk
        CHECK (char_length(zipcode) = 5);
    SQL
 
    add_column :users, :home_page_url, :string
    rename_column :users, :email, :email_address
  end
 
  def down
    rename_column :users, :email_address, :email
    remove_column :users, :home_page_url
 
    execute <<-SQL
      ALTER TABLE distributors
        DROP CONSTRAINT zipchk
    SQL
 
    drop_table :distributors
  end
end
```
Một hàm khác cũng khá hay dùng để rollback trong migration đó là hàm `revert`, khi gọi hàm này nó sẽ revert lại đúng những gì ta cung cấp (block hoặc class) giống với lệnh `rails db:rollback`
 
 ```ruby
 
require_relative '20121212123456_example_migration'
 
class FixupExampleMigration < ActiveRecord::Migration[5.0]
  def change
    revert ExampleMigration
 
    create_table(:apples) do |t|
      t.string :variety
    end
  end
end
```
File trên cũng có thể được viết lại mà không dùng `revert` nhưng ta phải để ý nhiều thứ hơn như là đổi lại thứ tự các lệnh trong hàm `change`, đổi ngược lại `up` và `down`, chuyển `create_table` thành `drop_table`, rắc rối hơn.
 
## Tổng kết

Trên là một vài điểm cần biết trước nhất về rails migration, như là công dụng cách viết, các lệnh hữu ích, rollback,... gọi là các lệnh hay dùng. Ngoài ra còn một vài thứ hay ho về migration như chạy migration cho các môi trường khác nhau, chạy phiên bản cố định migration, thay đổi output cho migration, cột modifier,... sẽ dành cho các bạn tìm hiểu sâu hơn.

Rất nhiều yêu thương 💖
