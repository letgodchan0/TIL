# 🌱 Django HTTP requests

> Django에서 HTTP 요청을 처리하는 방법
>
> 1. Django shortcut functions
> 2. View decorators



## 1. Django shortcuts functions

- `django.shortcuts` 패키지는 개발에 도움 될 수 있는 여러 함수와 클래스를 제공한다.

- `shortcuts function` 종류

  - `render()` - 이제까지 잘 쓰고 있던 `render`를 사용하지 않는다면 `templates`을 따로 받아줘야 한다.  즉 `render` 를 쓰면 복잡성 최소화 할 수 있음

  ![image-20220410131438653](Django%20-%20HTTP%20requests.assets/image-20220410131438653.png)

  - `redirect()`

  - `get_object_or_404()`

    - 모델 manager인 objects에서 `get()`을 호출하지만 해당 객체가 없을 경우, `DoesNotExist` 예외 대신 `Http 404` 를 화면에 띄워준다. 이게 무슨 말이냐면, 사용자들이 화면을 띄우려고 서버에 요청을 할 때  `get` 을 통해 서버에 요청이 이루어지는데, 만약 사용자가 원하는 화면이 존재하지 않는 화면이라면, 코드 실행 단계에서 해당 객체가 없기 때문에 500에러가 나가게 된다. 그런데 이런 상황은 사용자가 애초에 없는 페이지를 요청했기 때문에 발생한 상황으로 우리는 개발자 오류(?)인 `http status code 500`이 아닌 `404` 에러를 띄우는 것이 적절한 예외처리라고 할 수 있다.
    - 이렇게 상황에 따라 적절한 예외처리를 하고 클라이언튼에게 올바른 에러 상황을 전달하는 것 또한 개발의 중요한 요소 중 하나이다!

    ```python
    from django.shortcuts import get_object_or_404
    
    def detail(request, pk):
        articel = get_object_or_404(Article, pk=pk)
    ```

  - `get_list_or_404()`

## 2. Django View decorators

> Django는 다양한 HTTP 기능을 지원하기 위해 view 함수에 적용할 수 있는 여러 데코레이터를 제공한다. 

##### + [참고] 데코레이터 (Decorator)

- 어떤 함수에 기능을 추가하고 싶을 때, 해당 함수를 수정하지 않고 기능을 연장해주는 함수! 즉, 원본 함수를 수정하지 않으면서 추가 기능만을 구현할 때 사용

### Allowed HTTP methods

- 요청 메서드에 따라 `view` 함수에 대한 엑세스를 제한하며 요청이 조건을 충족시키지 못하면 `HttpResponseNotAllowed`을 반환 (405 Method Not Allowed)
- 한마디로, 해당 데코레이터가 요구하는 메서드가 아니라면 바로 밑에 함수를 실행하지 못하고 405 에러를 반환한다!
- HTTP 요청에 따라 적절한 예외처리 혹은 데코레이터를 사용해 서버를 보호하고 클라이언트에게 정확한 상황을 제공하는 것이 무엇보다 중요하다.

1. `require_http_methods()`

   - view 함수가 특정한 메서드 요청에 대해서만 허용하도록 하는 데코레이터 

   ```python
   # views.py
   from django.views.decorators.http import require_http_methods
   
   @require_http_methods(['GET', 'POST'])
   def create(request):
       pass
   ```

2. `require_POST()`

   - view 함수가 `POST` 메서드 요청만 승인하도록 하는 데코레이터

   ```python
   # views.py
   from django.views.decorators.http import require_POST
   
   @require_POST
   def delete(request, pk):
       pass
   ```

3. `require_safe()`

   - view 함수가 `GET` 및 `HEAD` 메서드만 허용하도록 요구하는 데코레이터
   - Django는 `require_GET` 대신 `require_safe`를 사용하는 것을 권장한다.

   ```python
   # views.py
   from django.views.decorators.http import require_safe
   
   @require_safe
   def index(request):
       pass
   ```

4. `delete view` 함수 변경

   - `@require_POST는` delete에서 쓰게 되는데 새로운 요청이 데코레이터를 뚫고 왔다면, 요청방식이 POST이기 때문에 `if`문이 필요가 없게 된다.
   -  또한 마지막 `detail`을 리턴해주는 것도 필요 없어진다. 왜냐면 POST가 아니면 기존처럼 detail을 띄워주는 것이 아니라(사용자는 삭제버튼을 눌렀는데 갑자기 웬 detatil이 나와.. 띠용?이어서) 405 에러를 띄워줄 것이기 때문에!

![image-20220410134638083](Django%20-%20HTTP%20requests.assets/image-20220410134638083.png)

