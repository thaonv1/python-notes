# Thao tác với model

## Định nghĩa model

```
from django.db import models

class MyModelName(models.Model):
    """A typical class defining a model, derived from the Model class."""

    # Fields
    my_field_name = models.CharField(max_length=20, help_text='Enter field documentation')
    ...

    # Metadata
    class Meta:
        ordering = ['-my_field_name']

    # Methods
    def get_absolute_url(self):
        """Returns the url to access a particular instance of MyModelName."""
        return reverse('model-detail-view', args=[str(self.id)])

    def __str__(self):
        """String for representing the MyModelName object (in Admin site etc.)."""
        return self.my_field_name
```

### Khai báo field

Mỗi một field là một trường trong db. Trong các app thì mỗi một class sẽ được lưu dưới db là tên một bảng được lưu theo dạng `app_class`. Ví dụ tên app là article và ta có 1 class model là category thì tên bảng ở csdl là `article_category`

`my_field_name = models.CharField(max_length=20, help_text='Enter field documentation')`

Ở trên ta có 1 field tên là `my_field_name` được lưu kiểu `varchar`

Ta có theo sau là một số argument, dưới đây là danh sách các arg có thể sử dụng

- `help_text`: Cung cấp cho user label để hiển thị thông tin giúp ng dùng biết họ đang lưu cái gì
- `verbose_name` : tên dưới dạng `human-readable`
- `default` : giá trị mặc định
- `null`: Nếu là `true` thì giá trị lưu dưới db là NULL. Mặc định là false
- `blank`: Nếu true thì nó cho phép trường này được lưu giá trị rỗng, mặc định sẽ là False.
- `choices` : Danh sách lựa chọn cho field này.
- `primary_key` : Set field này trở thành primary key

Một số field type phổ biến

- `CharField` dạng varchar, bắt buộc phải định nghĩa thêm option `max_length`
- `TextField` dạng text
- `IntegerField` dạng integer
- `DateField` và `DateTimeField` dạng datetime, ta có thể khai báo para `auto_now=True` hoặc `default`
- `EmailField` dạng email
- `FileField` hoặc `ImageField` được dùng để upload file và images. Nó sẽ có thêm một số validate để check image và một số para để định nghĩa file được lưu ở đâu và như nào
- `AutoField` là dạng `IntegerField` tự động tăng
- `ForeignKey` định nghĩa mối quan hệ one-to-many
- `ManyToManyField` định nghĩa quan hệ many-to-many

### Metadata

Có thể định nghĩa metadata trong class Meta. Một trong những tính năng thường sử dụng là quản lí cách sắp xếp mặc định của các bản ghi khi bạn query.

### Method

Model có thể có một số method. Bạn nên thêm phương thức `__str__()` vào tất cả các model để có thể đọc được dưới dạng string đối với mỗi object.

```
def __str__(self):
    return self.field_name
```

## Thao tác với db

- Tạo record

```
# Create a new record using the model's constructor.
record = MyModelName(my_field_name="Instance #1")

# Save the object into the database.
record.save()
```

- Search

`all_books = Book.objects.all()`

Ngoài ra ta sẽ có `filter`

```
wild_books = Book.objects.filter(title__contains='wild')
number_wild_books = wild_books.count()
```

Ta định nghĩa theo format

`field_name__match_type` (2 dấu gạch dưới)

Ta có một số  match type như sau:

- `contains`
- `icontains` (ko phân biệt hoa, thường)
- `iexact` (ko phân biệt hoa, thường)
- `exact`
- `in`
- `gt` (greater than)
- `startswith`

Để mở python shell

`python manage.py shell`

|Hàm|Mô tả|Ví dụ|
|---|-----|-----|
|filter()|trả về các phần tử phù hợp với đk|Reporter.objects.filter(full_name__startswith="si")|
|||Reporter.objects.filter(full_name__contains="ni")|
|exclude()|trả về các phần tử không phù hợp với đk|Reporter.objects.exclude(full_name__startswith="si")|
|||Reporter.objects.exclude(full_name__contains="ni")|
|||Reporter.objects.exclude(id=10)|
|order_by()|sắp xếp|Reporter.objects.filter(full_name__contains="ni").order_by('id')|
|reverse()|đảo|Reporter.objects.filter(full_name__contains="ni").order_by('id').reverse()|
|distinct()|lại bỏ dupplicate|Reporter.objects.filter(full_name__contains="ni").order_by('id').distinct('full_name')|
|all()|trả về tất cả các đối tượng|Reporter.objects.all()|
|create()|thêm đối tượng|Reporter.objects.create(full_name='Sinionth')|
|save()|lưu đối tượng|r = Reporter(full_name='Sinionth')|
|||r.save()|
|get()|lấy đối tượng|r=Reporter.objects.get(full_name__startswith="si")|
|get_or_create()|nếu đối tượng ko tồn tại thì tạo|r.objects.get_or_create(full_name="sinionth")|
|delete()|xóa đối tượng|r.delete()|
