### Mocking Django reverse ForeignKey relations in unit tests

If we have the following models,

```python
class Post(models.Model):
    title = models.CharField(max_length=255)
    text = models.TextField()

    def comment_count(self):
        return self.comments.count()

class Comment(models.Model):
    post = models.ForeignKey('Post', related_name='comments')
    message = models.TextField()
```

We can write a unit test for `Post.comment_count` by mocking Django's `ReverseManyToOneDescriptor`.

```python
from django.db.models.fields.related import ReverseManyToOneDescriptor

def test_post_comment_count(mocker):
    mock_queryset = mocker.Mock()
    mock_queryset.count.return_value = 3

    mock_get = mocker.patch.object(ReverseManyToOneDescriptor, '__get__')
    mock_get.return_value = mock_queryset

    post = Post(title='Test Post')
    assert post.comment_count == 3
```

If there are multiple related models, it's not ideal to mock all the relationships. So we can choose when to use the mocked queryset using `side_effect`.

Make sure to store the original function first before applying the patch.

```python
def test_post_comment_count(mocker):
    orig_get = ReverseManyToOneDescriptor.__get__
    mock_get = mocker.patch.object(ReverseManyToOneDescriptor, '__get__')

    mock_queryset = mocker.Mock()
    mock_queryset.count.return_value = 3

    def side_effect(*args, **kwargs):
        field = args[0].field
        related_name = field.related_query_name()
        if field.name == 'post' and related_name == 'comments':
            return mock_queryset
        return orig_get(*args, **kwargs)

   mock_get.side_effect = side_effect

   post = Post(title='Test Post')
   assert post.comment_count == 3
```
   
