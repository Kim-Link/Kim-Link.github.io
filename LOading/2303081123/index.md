---
layout: post
type: LOading
date: 2023-03-08 11:23
category: Python
title: "[Django 심화]Django select_related() prefetch_related()"
subtitle: 장고 쿼리셋 최적화
writer: 000000
post-header: true
header-img: ../django.png
hash-tag: [Python,Django]
---

## Intro

---

장고에 대해서 계속 공부중에 있다.

적응한거 같은데 아직도 잘 모르겠다. 후아후아…

이제 새로운 프로젝트가 시작되는데

오박사님이 select_related()와 prefetch_related()에 대해 공부해보라고 해서 정리한 내용을 끄적여 본다.


<br>


## Queryset

---

select_related()와 prefetch_related()에 대해 알기전에 Queryset에 대해 먼저 알아보자.

Django에서 QuerySet은 데이터베이스로부터 데이터를 검색, 필터링 및 정렬할 수 있는 강력한 도구다. 

QuerySet은 모델에 대한 쿼리를 실행한 결과이며, 데이터베이스와의 상호 작용을 추상화하여 더욱 편리하고 직관적인 데이터 조작을 가능하게 다.

QuerySet은 일반적으로 다음과 같은 방법으로 생성됩니다.

```python
# 모든 레코드를 가져오기
all_objects = MyModel.objects.all()

# 조건에 따라 레코드를 가져오기
filtered_objects = MyModel.objects.filter(field=value)

# 정렬된 레코드를 가져오기
ordered_objects = MyModel.objects.order_by('field')

# 일부 레코드를 가져오기
limited_objects = MyModel.objects.all()[:5]
```

QuerySet은 지연(lazy)된다. 즉, 데이터베이스에서 실제 데이터를 가져오기 전에는 쿼리가 실행되지 않는다. 대신, QuerySet을 다른 QuerySet 또는 리스트 등 다른 객체와 함께 사용할 수 있다. 

QuerySet은 항상 최신 데이터를 반영하며, 동일한 쿼리를 다시 실행할 때마다 새로운 결과를 반환한다.

QuerySet은 또한 체인(chain)이 가능하다. 이것은 하나의 QuerySet 객체를 다른 QuerySet 객체와 연결하여 하나의 쿼리로 실행할 수 있도록 해준다. 

이를 통해 여러 단계로 구성된 복잡한 쿼리를 작성할 수 있다. 

```python
# 여러 조건으로 필터링하고 정렬하기
queryset = MyModel.objects.filter(field1=value1).filter(field2=value2).order_by('field3')

# 필터링된 결과를 역순으로 정렬하기
reverse_queryset = queryset.reverse()

# QuerySet을 다른 QuerySet에 체인하기
chained_queryset = MyModel.objects.filter(field=value).exclude(field2=value2).order_by('-field3')
```

마지막으로, QuerySet은 값을 반환하는 다양한 메소드와 함께 제공된다. 

가장 일반적인 것은 **`.values()`**, **`.values_list()`** 메소드다. 

이들 메소드는 각 레코드의 필드 값을 가져올 수 있도록 해주는데, **`.values()`**는 딕셔너리 형태로 반환하고, **`.values_list()`**는 튜플 형태로 반환된다.


<br>



## select_related()

---

**`select_related()`**은 데이터베이스에서 관련된 객체를 미리 로드하여 성능을 향상시키는 메서드이다. 

이 메서드는 일반적으로 ForeignKey나 OneToOneField와 같은 ForeignKey 관계를 가진 모델에서 사용된다.

예를 들어, **`Blog`** 모델과 **`Entry`** 모델이 있고, **`Entry`** 모델이 **`Blog`** 모델을 ForeignKey로 참조하고 있다면 아래와 같이 작성할수 있다.

```python
class Blog(models.Model):
    name = models.CharField(max_length=100)
    tagline = models.TextField()

class Entry(models.Model):
    blog = models.ForeignKey(Blog, on_delete=models.CASCADE)
    headline = models.CharField(max_length=255)
    body_text = models.TextField()
    pub_date = models.DateTimeField()
    mod_date = models.DateTimeField()
    authors = models.ManyToManyField(Author)
```

이때, **`Entry`** 모델의 **`blog`** 필드에 대해 **`select_related()`**를 사용하여 **`Blog`** 모델을 미리 로드할 수 있다.

```python
entry = Entry.objects.select_related('blog').get(id=1)
```

이렇게 하면 **`entry.blog`**에 접근할 때마다 추가적인 데이터베이스 쿼리를 수행하지 않고, 이미 로드된 **`Blog`** 객체를 반환한다. 이는 대량의 데이터를 처리할 때 특히 유용하다.

<aside>
💡 참고로 `select_related()`는 일대다 관계(OneToMany)나 다대다 관계(ManyToMany)를 가진 모델에 대해서는 사용할 수 없습니다. 이 경우에는 `prefetch_related()` 메서드를 사용해야합니다.
</aside>


<br>


## prefetch_related()

---

**`prefetch_related()`** 메소드는 데이터베이스에서 관련된 객체를 불러올 때 사용된다. 이 메소드는 **`select_related()`**와는 다르게 "Many-to-Many" 또는 "One-to-Many"와 같은 관계에 대해서도 미리 가져올 수 있다. **`prefetch_related()`**를 사용하면 쿼리를 줄이고 성능을 향상시킬 수 있다.

다음은 **`prefetch_related()`**를 사용한 예제 코드다.

```python
from django.db import models

class Author(models.Model):
    name = models.CharField(max_length=100)

class Book(models.Model):
    title = models.CharField(max_length=200)
    author = models.ForeignKey(Author, on_delete=models.CASCADE)
    published_date = models.DateField()

class BookReview(models.Model):
    book = models.ForeignKey(Book, on_delete=models.CASCADE)
    reviewer_name = models.CharField(max_length=100)
    comment = models.TextField()
    rating = models.IntegerField()
```

위의 예제 코드에서 **`Author`** 모델과 **`Book`** 모델은 One-to-Many 관계를 가지고 있다. 그리고 **`Book`** 모델과 **`BookReview`** 모델은 One-to-Many 관계를 가지고 있다. 

위 모델에서 **`Author`** 객체와 함께 해당 **`Author`**의 **`Book`** 객체와 그 **`Book`**의 **`BookReview`** 객체를 가져 오려면 아래와 같이 불러올수 있다.

```python
from django.shortcuts import render
from django.db.models import Prefetch

def author_detail(request, author_id):
    author = Author.objects.get(id=author_id)
    books = Book.objects.filter(author=author).prefetch_related(
        Prefetch('bookreview_set', queryset=BookReview.objects.filter(rating__gte=3))
    )
    return render(request, 'author_detail.html', {'author': author, 'books': books})
```

위의 코드에서 author를 설명하자면 **`Author.objects`**는 **`Author`** 모델에 대한 쿼리셋(QuerySet)을 반환하고, **`.get(id=author_id)`**는 이 쿼리셋에서 **`id`** 필드가 **`author_id`**인 객체를 가져온다.

**즉, `Author` 모델에서 `id`가 `author_id`와 같은 객체를 가져온다.** 이 메소드는 객체가 존재하지 않거나, 여러 개의 객체가 존재할 경우 **`DoesNotExist`**
 또는 **`MultipleObjectsReturned`** 예외를 발생시킵니다.

<aside>
💡 `.get()` 메소드는 Django의 ORM(Object Relational Mapping)에서 데이터베이스로부터 단일 객체를 가져올 때 사용됩니다.
</aside>

book은 **`Book`** 모델에서 **`author`** 필드가 현재 **`author`** 객체와 같은 책들을 가져온 후, 그 책들과 연결된 **`BookReview`** 객체를 가져오는데, 이 때 **`rating`**이 3 이상인 것만 가져오도록 필터링한 것이다.

**`prefetch_related()`** 메소드는 이전에 쿼리셋에서 가져온 객체와 연결된 객체를 미리 불러와서 캐싱해 둠으로써, 추가적인 쿼리를 최소화하여 성능을 향상시킨다. 

즉, **`Book`** 모델의 쿼리셋을 가져온 후 **`BookReview`** 모델과의 관계를 가진 객체들도 미리 가져와서 캐싱해두고, 나중에 이에 접근할 때는 추가적인 쿼리 없이 미리 가져온 데이터를 사용한다.

**`Prefetch()`** 메소드는 **`prefetch_related()`** 메소드와 함께 사용된다. 이 메소드는 연결된 객체를 불러오는데 사용되며, 필요한 경우에는 연결된 객체를 필터링할 수 있다. 예제에서는 **`Prefetch()`** 메소드를 사용하여 **`Book`** 모델의 **`bookreview_set`** 필드를 불러오는데, 이 때 **`queryset`** 매개변수를 사용하여 **`rating`**이 3 이상인 **`BookReview`** 객체들만 가져오도록 필터링했다.

따라서, 이 코드를 실행하면 **`author`** 객체와 관련된 책들을 가져오는데, 이들과 연결된 **`BookReview`** 객체들 중에서 **`rating`**이 3 이상인 것들만 가져와서 함께 반환된다. 

이렇게 함으로써 성능이 향상되는데, **`Book`** 모델과 **`BookReview`** 모델의 One-to-Many 관계에서 많은 수의 **`Book`** 객체와 **`BookReview`** 객체가 있는 경우, 추가적인 쿼리를 하지 않고도 연관된 객체를 미리 가져와서 처리할 수 있기 때문이다.

**`prefetch_related()`**를 사용하여 쿼리를 최적화하면 데이터베이스에서 데이터를 불러오는 속도를 향상시킬 수 있다. 또한, **`select_related()`**와 같은 메소드와는 달리 "Many-to-Many" 또는 "One-to-Many"와 같은 관계도 미리 가져올 수 있다는 장점이 있다.



<br>




## 메모리 사용량이 많은가?

---

메모리 사용량을 늘려서 쿼리의 속도를 빠르게 한다는것은 이해가 된다. 그렇다면 메모리가 얼마나 많이 사용되는지 고민해 보아야 한다. 서버가 감당할수 없늘정도의 메모리를 사용한다면 오히려 사용하지 않는것이 더 좋을수도 있다.

그리고 select_related()와 prefetch_related() 메서드의 메모리 사용에 대한 차이도 고민해볼 필요가 있다.

`select_related()` 메서드는 ForeignKey나 OneToOneField와 같은 단일 관계필드를 사용하는 경우, 해당 필드와 관련된 모든 객체를 가져오기 때문에 **메모리 사용량이 크게 증가하지 않습니다.** 

`prefetch_related()` 메서드는 ManyToManyField와 같은 다중 관계필드를 사용하는 경우, 해당 필드와 관련된 모든 객체를 가져오기 때문에 메모리 사용량이 더 많아질 수 있습니다. 그러나 prefetch_related() 메서드는 지연로딩(lazy loading)을 사용하여 필요한 시점에 객체를 가져오기 때문에, 일반적인 **queryset을 사용하는 것보다 메모리 사용량이 크게 증가하지 않습니다.**

단적인 예로 “Many-to-Many"가 3개정도 있는 서비스에서 prefetch_related()를 사용하면 일반적인 queryset을 사용하는 것보다 메모리 사용량이 미세하게 늘어날 수 있지만, 일반적인 queryset을 사용하는 것보다 쿼리의 실행 시간이 매우 단축될 것으로 예상된다. 이에 따라, 실제로는 prefetch_related()를 사용하는 것이 성능 향상에 큰 도움이 될 수 있다.


<br>




## Outro

---

뭐 위의 글에는 엄청 좋은것 처럼 써놧지만, 정확하게 알지 못하는 경우에 대해서 경계해야 한다.

중복 쿼리를 줄여 속도를 올리고, 메모리를 일반적인 쿼리셋보다는 많이 사용하지만 조금더 사용함으로써 많은 속도 향상을 볼수 있다곤 하지만,

정말 중복 쿼리가 없는지, 혹은 db 구조가 너무 많이 복잡해 굳이 사용하지 않아도 되는 메모리까지 사용하진 않는지 고민해볼 필요는 있다.

무조건 좋은것은 없다.

조건부 좋은것은 많으니 조건을 따져보고 사용하도록 하자.


<br>


[참고 문헌]

[Django - Django select_related() prefetch_related()](https://leffept.tistory.com/312)