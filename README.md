## Criação de Base de dados, admin e models e seu Admin.

#### 1º Passo.
   * Pip install Django
   * Pip install pillow
   * Pip install mysqlclient
---
#### 2º Passo.
Iniciando o projeto
   * Django-admin startproject __blog__ .  <( ponto no final é necessario para nao acriar duas pastas )
Esta sera a pasta Mean do programa.

Blog sera sua pasta principal

---
#### 3º Passo.
Criando os apps
Craicao de sub pastas, para a divisao de seu site. Entre no terminal e digite.

Categorias
   * python manage.py startapp categorias
   
Post
   * python manage.py startapp posts
   
Comentarios
   * python manage.py startapp comentarios

Em Setting da pasta principal __Blog__ - INSTALLED_APPS

Adicione nas primeiras linhas como:

INSTALLED_APPS = [
```
‘categotias’ , 
‘posts’ ,
‘comentarios’ ,
```

---
#### 4º Passo.
settings da pasta principal __Blog__
adicione. Onde vamos criar a pasta e colocar nossos HTMLS futuramente.

TEMPLATES in ‘DIRS’ [``` os.path.join(BASE_DIRS, ‘ templates’)``` ],

---
#### 5º Passo.
Crete a base de dados mysql no workmench, com a descrição.

Nome = blog-jango
utf8mb4_ general_ci

__Blog__ - settings - DATABASE

```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'blog-django',
        'HOST': '127.0.0.1',
        'PORT': '3306',
        'PASSWORD': '123',
    }
}
```
Cheque seu __Host__ e __Post__

---
#### 6º Passo.
Mudando a linguagem do site para PT/Brasil

```
LANGUAGE_CODE = 'pt-BR'
TIME_ZONE = 'America/Sao_Paulo'
USE_I18N = True
USE_L10N = True
USE_TZ = True
```

---
#### 7º Passo.
Adcionando as medias e statics no __Blog__ settings, nas ultimas linhas do script. Isso fara a leitura e direcionamento das pastas futuras quando criado o __templates__ e seu __static__.

```
STATICFILES_DIRS = (os.path.join(BASE_DIR, ‘templates/static’),)
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
MEDIA_URL = '/media/'
```
---
#### 8º Passo.
__Blog__ em __urls.py__ - Criadção das __urlpatterns__.

```
from django.contrib import admin
from django.urls import path, include
from django.conf import settings
from django.conf.urls.static import static
urlpatterns = [
    path('', include('posts.urls')),
    path('admin/', admin.site.urls),
]
urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA.ROOT)
```
__path('', include('posts.urls'))___ Este sera sua pagina principal
__path('admin/', admin.site.urls)___ Este sera sua pagina principal do admin

---
#### 9º Passo.
__BLOG_ - __SETTINGS.py__ - Mensagens de sucesso ou de erro.

from django.contrib.messages import constants

```
MESSAGE_TAGS = {
    constants.ERROR: 'alert-danger',
    constants.WARNING: 'alert-warning',
    constants.DEBUG: 'alert-info',
    constants.SUCCESS: 'alert-success',
    constants.INFO: ‘alert-info',}
```

---
#### 10º Passo.
__post__ - __ urls.py__  / Criacao de views 
Também precisamos da criação da pasta URLS em __ posts__, Se criar as URLS precisa criar as VIEWS, uma depende da outra( em POSTS a URLS que se liga pela VIEWS )

Dentro de __ URLS.py__ 

```
from django.urls import path
from . import views

urlpatterns = [
    path('', views, name='index'),
    path('categoria/<str:categoria>', views, name='post_categorias'),
    path('busca/', views, name='post_busca'),
    path('post/<int:pk>', views, name='post_detalhes'),
]
```

    * 'categoria/<str:categoria>'
str / de categorias == aparecera na barra de busca busca
    * 'post/<int:pk>'
int = numero / Primmer Key ou ID == aparecera na barra busca

---
#### 11º Passo.
__post__ em __views.py__ - Adicionamos classes, por enquanto sem funções:

```
from django.views.generic import ListView
from django.views.generic.edit import UpdateView

class PostIndex(ListView):
    pass

class PostBusca(PostIndex):
    pass

class PostCategorias(PostIndex):
    pass

class PostDetalhes(UpdateView):
    pass
```

---
#### 12º Passo.
__post__ em __urls.py__
Os VIEWS devem ser chamados do __post__ em __views.py__ para esta pasta __urls.py__ ligando eles e para isso usamos __calabos__.

```
e assim editamos adicionado em todas as views com as respectivas classes
urlpatterns = [
    path('', views.PostIndex.as_view(), name='index'),
    path('categoria/<str:categoria>', views.PostCategorias.as_view(), name='post_categorias'),
    path('busca/', views.PostBusca.as_view(), name='post_busca'),
    path('post/<int:pk>', views.PostDetalhes.as_view(), name='post_detalhes'),
```

Calabos:
    * views.PostIndex.as_view()
    
---
#### 13º Passo.
__Models__ e __Admin__ em __Categorias__.
Categorias em __models.py__ / criação de models adicionando banco de dados.
```
class Categoria(models.Model):
    nome_cat = models.CharField(max_length=50)

def __str__(self):
    return self.nome_cat
```
Ao invés de aparecer o nome do objeto sera mostrado o nome da categoria por isso usamos do ```def __str__(self): return self.nome_cat ```

    * Ligando isso agora no Admin
Categoria em __admin.py__ 

```
from django.contrib import admin
Importando models de categorias
from .models import Categoria
Fazendo outra classe agora para o admin
class CategoriaAdmin(admin.ModelAdmin):
    list_display = ('id', 'nome_cat')
    list_display_links = ('id', 'nome_cat')

registrando ambos do passo 13 models.py e admin.py
admin.site.register(Categoria, CategoriaAdmin)
```

---
#### 14º Passo.
__Models__ e __Admin__ / __Posts__
Mesmo seguimento do passo 13
Fazendo o __model.py__ e em seguida anexando no __admin.py__

Posts / __models.py__

```
from categorias.models import Categoria
Todos os models se ligam de alguma maneira, fazendo a herança de Categoria
from django.contrib.auth.models import User
from django.utils import timezone

class Post(models.Model):
    titulo_post = models.CharField(max_length=255, verbose_name='Titulo')
    autor_post = models.ForeignKey(User, on_delete=models.DO_NOTHING, verbose_name='Autor')
    data_post = models.DateTimeField(default=timezone.now, verbose_name=‘Data')
    conteudo_post = models.TextField(verbose_name='Conteudo')
    exerto_post = models.TextField(verbose_name='Exerto')
    categoria_post = models.ForeignKey(Categoria, on_delete=models.DO_NOTHING, blank=True, null=True, verbose_name='Categoria')
    imagem_post = models.ImageField(upload_to='post_img/%Y/%m?%d', blank=True, null=True, verbose_name=‘Imagem')
Imagem dia mes e ano e direcionando onde irar salvas (upload_to=‘post_img/%Y/%m?%d')
    publicado_post = models.BooleanField(default=False, verbose_name='Publicado')

    def __str__(self):
        return self.titulo_post
```    

     * verbose_name=' ‘ 
transforma o nome para o escolhido)

    * (upload_to=‘post_img/%Y/%m?%d') 

Imagem dia mes e ano e direcionando onde irar salvas 


__Post__ / __admin.py__
Registrando a class
Repetindo a formula do categorias/__admin.py__ aqui no Post/__admin.py__

```
from django.contrib import admin
from.models import Post

class PostAdmin(admin.ModelAdmin):
    #quais os campos para serem exibidos
    list_display = ('id', 'titulo_post', 'autor_post',
                    'categoria_post', 'publicado_post')
    list_editable = ('publicado_post',)
    list_display_links = ('id', 'titulo_post',)

admin.site.register(PostAdmin, Post)
```

---
#### 15º Passo.
##### Vamos ver se ate aqui esta ocorrendo tudo corretamente realizando a função Makemigrationse Migrate no terminal.

    * Python manege.py makemigrations
Obs: se over algum erro recomendo uma pesquisa no google, ai sera testada suas skills de deiteive.

Em seguida
    * python manage.py migrate
Ira fazer todas as migrações

---
#### 16º Passo.
Criação do espaço do admin, para isso usamos o __Superuser__ abrindo novamento o terminal.

    * python manage.py createsuperuser
Apos isso digite seu nome de __user__ / __email__ / __senha__ / __confirme a senha__

    * pathon manage.py run server
Para fazer rodar o seridor

>Agora deve clicar no link do servidor ( lembrando a pagina nao estará pronta ) No link a cima __http://127.0.0.1:8000__ deve ser adicionado __/admin__ para >entrar na area do admin do server ( __http://127.0.0.1:8000/admin__ ) inserindo o usuario e senha criado anteriormente.

---
#### 17º Passo.
Comentarios __models.py__ e __admin.py__, ligando eles assim com no passo __13__, __14__.

Comentarios / __models.py__
Adicionando no comentario __Nome__ / __Email__ / __tamanho do texto__ / __usuario__ / __data__ / __Publicação.__

```
from django.db import models
from posts.models import Post
Aqui herdamos de Post/models.py
from django.contrib.auth.models import User
from django.utils import timezone

class Comentario(models.Model):
    nome_comentario = models.CharField(max_length=20)
    email_comentario = models.EmailField()
    comentario =models.TextField(max_length=150)
    post_comentario = models.ForeignKey(Post, on_delete=models.CASCADE)    
Quando um post for deletado todos os comentários sera apagador
    usuario_comentario = models.ForeignKey(User, on_delete=models.DO_NOTHING)
    data_comentario = models.DateTimeField(default=timezone.now)
    publicado_comentario = models.BooleanField(default=False)

def __str__(self):
    return self.nome_comentario
```

Comentarios / __admin.py__. Quais campos dever serem exibidos no admin do site.

```
from django.contrib import admin
from .models import Comentario 

class ComentariosAdmin(admin.ModelAdmin):
    list_display = ('id', 'nome_comentario', 'email_comentario',
                    'post_comentario', 'data_comentario',
                    'publicado_comentario')
    list_editable = ('publicado_comentario',)
    list_display_links = ('id', 'nome_comentario', 'email_comentario',)


admin.site.register(Comentario, ComentariosAdmin)
```
    * display
>Sera mostrado.
    * Editable
>Sera editado
    * display_link
>Sera mostrado e sera um clic link para entrar 

Como feito anteriormente no terminal, __python manage.py makemigration__ em seguida __python manage.py migrate__
>Com isso ja sera possível ver as alterações na pagina admin faca um teste e crie um post.

---
#### 18º Passo.

__Blog__ em __settings.py__ adicionar ultimas linhas o __Summernote__ ( Editor de testo ).

```
INSTALLED_APPS += ('django_summernote', )
```
    * Caso seja Mac adicione junto
```
X_FRAME_OPTIONS = 'SAMEORIGIN' 
```

__Blog__ / __urls.py__.
```
path('summernote/', include('django_summernote.urls')),
```

__post__ / __admin.py__. Adicione.
```
from django_summernote.admin import SummernoteModelAdmin

summernote_fields = ('conteudo_post',)
```
>__Conteudo_post__ e onde eu quero que seja adicionado o meu editor de testo __Summernote__.

---

















