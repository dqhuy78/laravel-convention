## 1. Commands

Các command tạo ra không cần có tiền tố `...Command` trong tên class và tên file. Ví dụ:

<style>i{color:red;}</style>
<i>// Bad</i>

```php
// app/Console/Commands/DoSomeThingCommand.php

class DoSomeThingCommand extends Command
{
    //
}
```

<style>em{color:green;}</style>
<em>// Good</em>

```php
// app/Console/Commands/DoSomeThing.php

class DoSomeThing extends Command
{
    //
}
```

## 2. Enums

Các biến dạng hằng số sử dụng ở nhiều chỗ khác nhau thì cần tạo một file riêng để lưu trữ các biến này để có thể tái sử dụng về sau. Ví dụ: ta có 2 role là `admin` và `member`, thay vì code như sau:

<i>// Bad</i>

```php
$user->update(['role' => 'admin'])
```

Thì ta tạo một file mới như sau:

```php
// app/Enums\RoleEnum.php

final class Role
{
    const ADMIN = 'admin';
    const MEMBER = 'member';
}
```

Và sử dụng lại:

<em>// Good</em>

```php
use App\Enums\Role;

$user->update(['role' => Role::ADMIN])
```

> Lưu ý: Đặt tên class Enum cần tránh trường hợp bị trùng với tên Model.

## 3. Controllers

Sẽ có 2 loại controller là Restful Controller và Invoke Controller.

### a. Restful Controller

Là controller chỉ chứa các **public method** tương ứng với `Restfull`. Ví dụ

```php
class PostController extends Controller
{
    public function index()
    {
        //
    }

    public function show()
    {
        //
    }

    public function create()
    {
        //
    }

    public function store()
    {
        //
    }

    public function edit()
    {
        //
    }

    public function update()
    {
        //
    }

    public function destroy()
    {
        //
    }
}
```

### b. Invoke Controller

Đối với các tính năng nằm ngoài `Restfull` như nói trên thì ta sẽ tạo một controller tương ứng với tính năng đó và đặt trong folder tương ứng với Model mà tính năng đó thực hiện. Giả sử, ta muốn thêm một tính năng là thêm tag vào một bài viết, thì ta sẽ tạo một invoke controller như sau:

```php
// app\Http\Controllers\Post\AddTag.php

class AddTag extends Controller
{
    public function __invoke()
    {
        //
    }
}
```

Invoke controller không cần hậu tố `Controller` trong tên class và này chỉ được chứa duy nhất một public method là `__invoke()`, duy nhất một tính năng.

## 4. Middleware

Các middleware tạo ra không cần có tiền tố `...Middleware` trong tên class và tên file. Ví dụ:

<i>// Bad</i>

```php
// app/Http/Middleware/VerifyAdminMiddleware.php

class VerifyAdminMiddleware
{
    //
}
```

<em>// Good</em>

```php
// app/Http/Middleware/VerifyAdmin.php

class VerifyAdmin
{
    //
}
```

## 5. Requests

- Đối với class request dành cho model nào thì cần đặt vào trong model đó và nên sử dụng hậu tố `...Request` trong tên class để dễ phân biệ trong code.
- Trường hợp một request không xác định được dành cho model nào thì có thể để luôn trong folder gốc `app/Http/Requests/`.
- Ví dụ một request validate cho việc tạo post sẽ có dạng:

```php
// app\Http\Requests\Post\StorePostRequests.php

class StorePostRequest extends FormRequest
{
    //
}
```

## 6. Resources

- Mỗi model chỉ nên có 1 Resource class tương ứng thay vì nhiều Resource class. Trong trường hợp bắt buộc cần tạo 2 Resource class cho cùng một model cần bàn lại với team.
- Mỗi Resource cần có hậu tố `...Resource` trong tên class để phân biệt với model tương ứng.
- Trường hợp dữ liệu trả về cần phân biệt theo role thì có thể sử dụng các hàm như `->whenMerge()` để làm điều này.
- Ouput key của Resource nên ở dạng `camelCase`, ví dụ:

<i>// Bad</i>

```php
// app/Http/Resources/PostResource.php

class PostResource extends ShareResource
{
    public function toArray()
    {
        return [
            'id' => $this->id,
            'title' => $this->title,
            'is_publish' => $this->is_publish // Bad
        ]
    }
}
```

<em>// Good</em>

```php
class PostResource extends ShareResource
{
    public function toArray()
    {
        return [
            'id' => $this->id,
            'title' => $this->title,
            'isPublish' => $this->is_publish // Good
        ]
    }
}
```

## 7. Job, Event, Listener

- 3 loại nói trên cũng cần đặt vào folder tương ứng với model hoặc nhóm mà nó tương tác với và cũng nên đặt thêm hậu tố `...Job`, `...Evenet`, `...Listener`
- Trường hợp không xác định dành cho model hoặc nhóm nào cụ thể thì có thể để luôn ở folder gốc

```php
// Job
class RemovePostRelationJob
{

}

// Event
class NewPostPublishEvent
{

}

// Listener
class NewPostPublishListener
{

}
```

## 8. Models

- Các models sẽ được đặt trong folder `app/Models/`.
- Trường hợp model có quá nhiều `scope` hoặc  các helper function như `isPublish()`, `isDraft()` thì có thể tách các scope, helper function này ra thành 1 Trait riêng và sao đó use lại vào model.
- Các trait này sẽ đặt trong 2 foldẻ là `app/Models/Scopes/` (đối với custom scope) hoặc `app/Models/Helpers/` (đối với helper).


## 9. Policies

- Policy cho các model đặt luôn trong folder gốc `app/Policies/` và phải có hậu tố `Policy` để tránh trùng với tên model.

## 10. Services

- Đối với các class có các function dùng để tương tác với bên thứ 3 thì nên đặt vào folder `app/Services/` và tên class cũng nên có từ `Service`. Ví dụ:

```php
// app/Services/GoogleSheetService.php

class GoogleSheetService
{

}
```
