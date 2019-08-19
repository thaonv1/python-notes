# Note về class base view

Ở mức độ đơn giản nhất, class based view giúp bạn tối giản hóa các thao tác làm việc với http method, tức là thay vì bạn phải dùng code để xử lí thì nó đã có sẵn các class tự động nhận diện.

Ví dụ

```
from django.http import HttpResponse
from django.views import View

class MyView(View):
    def get(self, request):
        # <view logic>
        return HttpResponse('result')
```

Và bởi vì thực tế thì url sẽ mong đợi 1 function được gọi sau khi match chứ không phải class nên class based view có sẵn `as_view` method. Function này sẽ tạo ra 1 instance của class rồi gọi tới các method của nó như `setup()`để thiết lập params hoặc `dispatch` để xem xét http request...

## Sử dụng mixins

Mixins là tập hơn của một loạt các thuộc tính và phương thức được kế thừa từ các class cha. Hiểu một cách đơn giản thì nó sẽ dùng để thêm vào quá trình gọi các function của class. 
