# NewsportalSkillfactory
Database, DjangoORM task
Console commands :
# 1. Создать двух пользователей
from django.contrib.auth.models import User
user1 = User.objects.create_user(username='user1')
user2 = User.objects.create_user(username='user2')
# 2. Создать два объекта модели Author, связанные с пользователями
from news.models import Author
author1 = Author.objects.create(user=user1)
author2 = Author.objects.create(user=user2)
# 3. Добавить 4 категории в модель Category
from news.models import Category
category1 = Category.objects.create(title='Category1')
category2 = Category.objects.create(title='Category2')
category3 = Category.objects.create(title='Category3')
category4 = Category.objects.create(title='Category4')
# 4. Добавить 2 статьи и 1 новость
from news.models import Post
post1 = Post.objects.create(author=author1, post_type=Post.ARTICLE, title='Article1', content='Content of Article1')
post2 = Post.objects.create(author=author1, post_type=Post.ARTICLE, title='Article2', content='Content of Article2')
post3 = Post.objects.create(author=author2, post_type=Post.NEWS, title='News1', content='Content of News1')
# 5. Присвоить им категории
from news.models import PostCategory
PostCategory.objects.create(post=post1, category=category1)
PostCategory.objects.create(post=post1, category=category2)
PostCategory.objects.create(post=post2, category=category3)
PostCategory.objects.create(post=post3, category=category1)
PostCategory.objects.create(post=post3, category=category4)
# 6. Создать 4 комментария к разным объектам модели Post
from news.models import Comment
comment1 = Comment.objects.create(post=post1, user=user1, text='Comment1 on Article1')
comment2 = Comment.objects.create(post=post1, user=user2, text='Comment2 on Article1')
comment3 = Comment.objects.create(post=post2, user=user1, text='Comment1 on Article2')
comment4 = Comment.objects.create(post=post3, user=user2, text='Comment1 on News1')
# 7. Применить функции like() и dislike() к статьям/новостям и комментариям
# Лайки и дизлайки постов
post1.like()
post2.like()
post1.like()
post2.dislike()
post3.like()

# Лайки и дизлайки комментариев
comment1.like()
comment2.like()
comment2.like()
comment3.dislike()
comment4.like()
# 8. Обновить рейтинги пользователей
author1.update_rating()
author2.update_rating()
# 9. Вывести username и рейтинг лучшего пользователя
best_author = Author.objects.order_by('-rating').values('user__username', 'rating')[0]
print(f"Лучший пользователь: {best_author['user__username']}, рейтинг: {best_author['rating']}")
# 10. Вывести дату добавления, username автора, рейтинг, заголовок и превью лучшей статьи
best_article = Post.objects.filter(post_type=Post.ARTICLE).order_by('-rating').values('creation_date_time', 'author__user__username', 'rating', 'title', 'content')[0]
preview = best_article['content'][:124] + '...'
print(f"Лучшая статья: {best_article['title']}, автор: {best_article['author__user__username']},"
      f" рейтинг: {best_article['rating']}, дата: {best_article['creation_date_time']}, превью: {preview}")
  
 # 11. Вывести все комментарии к этой статье
best_article_comments = Comment.objects.filter(post_id=best_article['id']).values('creation_date_time', 'user__username', 'rating', 'text')

print("Комментарии к лучшей статье:")
for comment in best_article_comments:
    print(f"Дата: {comment['creation_date_time']}, пользователь: {comment['user__username']}, рейтинг: {comment['rating']}, текст: {comment['text']}")
